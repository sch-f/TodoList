<template>
    <li>
        <label>
            <input type="checkbox" :checked='yigshi.done' @click="handleCheck(yigshi.id)" />
            <span v-show="!yigshi.isEdit">{{ yigshi.titel }}</span>   <!-- 通过v-show来控制时span显示还是input显示 -->
            <input v-show="yigshi.isEdit" type="text" :value="yigshi.titel" @blur="handleBlur(yigshi,$event)"> <!-- 失去焦点时 -->
        </label>
        <button class="btn btn-danger" @click="delTodo(yigshi.id)">删除</button>
        <button class="btn btn-danger" style="margin-right: 10px;" 
        @click = 'handleEdit(yigshi)'
        v-show="!yigshi.isEdit">编辑</button>  <!-- input框显示时,按钮不显示 -->
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
            
        },   
             //编辑todo item 
        handleEdit(yigshi){
            //如果yigshi里面有isEdit则直接修改其值,反之给他添加一个isEdit
            if (yigshi.hasOwnProperty('isEdit')) { //hasOwnProperty判断一个对象是否拥有这个属性
                yigshi.isEdit = true
            }else{
                this.$set(yigshi,'isEdit',true) //给item添加isEdit属性
            }
            
        },
        //通过失去焦点,来修改item里面的内容
        handleBlur(yigshi,e){    //e时事件对象 $event
            yigshi.isEdit = false;
            console.log('updateTodo',yigshi.id,e.target.value);
            //校验input框是否有内容
            if(!e.target.value.trim())return alert('输入不能为空')
            this.$bus.$emit('updateTodo',yigshi.id,e.target.value) //获取input框中输入的内容,传递给App组件
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