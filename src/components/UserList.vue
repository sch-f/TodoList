<template>
    <ul class="todo-main">
        <!-- 加:是表达式   不加就是一个属性 -->
        <UserItem v-for="todoObj in todos" 
            :key="todoObj.id" 
            :yigshi='todoObj' 
            ></UserItem><!-- 把checkedTodo传给item -->
    </ul>
</template>

<script scoped>
import UserItem from './UserItem.vue'
export default {
    name: 'UserList', //组件名
    components: { UserItem }, //注册UserItem组件
    props: ['todos' ]//接受App组件传过来的todos对象
}
</script>

<style scoped>
/*main*/
.todo-main {
    margin-left: 0px;
    border: 1px solid #ddd;
    border-radius: 2px;
    padding: 0px;
}

.todo-empty {
    height: 40px;
    line-height: 40px;
    border: 1px solid #ddd;
    border-radius: 2px;
    padding-left: 5px;
    margin-top: 10px;
}
</style>