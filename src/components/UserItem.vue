<template>
    <li>
        <label>
            <input type="checkbox" :checked='yigshi.done' @click="handleCheck(yigshi.id)" />
            <span>{{ yigshi.titel }}</span>
        </label>
        <button class="btn btn-danger" @click="delTodo(yigshi.id)">删除</button>
    </li>
</template>

<script scoped>
import pubsub from 'pubsub-js' 
export default {
    name: 'UserItem',
    props: ['yigshi'],  //收到List传过来的yigeshi对象,checkedTodo函数,deletTodo函数
    methods: {
        //id来自yigshi.id
        handleCheck(id) {
            //通知App组件将对应的事件取反
            pubsub.publish('checkedTodo',id)    //发布消息,参数1:发布消息的名字,参数2:携带的数据
        },
        //删除todo
        delTodo(id){
            if(!confirm('确定删除吗'))return
            pubsub.publish('deletTodo',id)
            
        }
    }
}
</script>

<style scoped>
/*item*/
li {
    list-style: none;
    height: 36px;
    line-height: 36px;
    padding: 0 5px;
    border-bottom: 1px solid #ddd;
}

li label {
    float: left;
    cursor: pointer;
}

li label li input {
    vertical-align: middle;
    margin-right: 6px;
    position: relative;
    top: -1px;
}

li button {
    float: right;
    display: none;
    margin-top: 3px;
}

li:before {
    content: initial;
}

li:last-child {
    border-bottom: none;
}

li:hover {
    background-color: rgb(122, 210, 128);
}

li:hover button {
    display: block;
} 
</style>