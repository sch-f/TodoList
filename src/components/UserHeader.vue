<template>
    <div class="todo-header">
        <input type="text" placeholder="请输入你的任务名称，按回车键确认" @keyup.enter="add"/>
    </div>
</template>

<script>
import {nanoid} from 'nanoid'
export default {
    name: 'UserHeader',
    //props:['addTodo'], //改为自定义事件后,就不用接收数据
    methods:{
        add(event){
            if(!event.target.value.trim())return   //校验内容为空,直接返回出去
            //将用户输入包装为todo对象  传给App组件
            const todoObj = {
                id:nanoid(),titel:event.target.value,done:false
            }
            //rective函数是App组件传过来的,在vc里面,把todoObj作为函数的参数
            //this.addTodo(todoObj)  //由于改为了自定义事件就不这样调用
            this.$emit('addTodo',todoObj)//参数1:需要触发的事件,参数二.触发事件的参数
            event.target.value=''//输入完成后清空input
        },
        
    }
}
</script>

<style scoped>
/*header*/
.todo-header input {
    width: 560px;
    height: 28px;
    font-size: 14px;
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 4px 7px;
}

.todo-header input:focus {
    outline: none;
    border-color: rgba(82, 168, 236, 0.8);
    box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
}


</style>