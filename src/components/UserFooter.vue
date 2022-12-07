<template>
    <div class="todo-footer" v-show="todos.length"> <!-- 当没有一个todo时，footer不显示  todos.length为0则为false -->
        <label>
            <input type="checkbox" :checked="isAll" @click="ckAll" />
        </label>
        <span>
            <span>已完成{{ completeTodo }}</span> / 全部{{ todos.length }}
        </span>
        <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
    </div>
</template>

<script>
export default {
    name: 'UserFooter',
    props: ['todos','checkedAll','cleartodo'],
    computed: {
        //已完成，被勾选的todo数量
        completeTodo() {
            let j = 0
            this.todos.forEach(todo => {
                if (todo.done) j++
            });
            return j
        },
        //通过 勾选选项和全部选项是否相等来控制全选按钮
        isAll() {
            return this.completeTodo == this.todos.length && this.completeTodo != 0   //&&后面是为了已完成等于0时 不勾选
        }
    },
    methods: {
        //实现全选
        ckAll(e) {
            //使用自定义事件
            this.$emit('checkedAll',e.target.checked)//e.target.checked获取当前checked的状态  App传过来的checkedAll函数
        },
        //删除已完成的任务  点击一次就运行一次cleartodo（）函数
        clearAll(){
            //使用自定义事件
            this.$emit('cleartodo')
        }
    }
}
</script>

<style scoped>
/*footer*/
.todo-footer {
    height: 40px;
    line-height: 40px;
    padding-left: 6px;
    margin-top: 5px;
}

.todo-footer label {
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
}

.todo-footer label input {
    position: relative;
    top: -1px;
    vertical-align: middle;
    margin-right: 5px;
}

.todo-footer button {
    float: right;
    margin-top: 5px;
}
</style>