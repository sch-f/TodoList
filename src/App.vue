<template>
  <div id="app">
    <div id="root">
      <div class="todo-container">
        <div class="todo-wrap">
          <!-- 改为自定义事件 -->
          <UserHeader @addTodo="addTodo"></UserHeader>
          <UserList :todos="todos" ></UserList>
          <!-- 传送数据,不能使用自定义事件 -->
          <UserFooter :todos="todos"  @checkedAll="checkedAll" @cleartodo="cleartodo"></UserFooter>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import UserFooter from './components/UserFooter.vue';
import UserHeader from './components/UserHeader.vue';
import UserList from './components/UserList.vue';
import pubsub from 'pubsub-js'   //引入消息订阅的库,所有框架都可以使用 npm i pubsub-js@1.6.0
export default {
  name: 'App',
  components: {
    UserFooter, UserHeader, UserList
  },
  data() {
    return {//从浏览器本地存储拿到todos            [] 那个为真footer的length使用那个,
      todos:JSON.parse(localStorage.getItem('todos')) || []/*  [    测试用'数据'  //把todos传给List
        { id: '001', titel: '吃饭', done: true },
        { id: '002', titel: '睡觉', done: false },
        { id: '003', titel: '游戏', done: true }
      ] */
    }
  },
  methods: {
    //添加一个todo
    //接收到Header传过来的todo参数
    addTodo(x) {
      console.log('我是app,接受到:', x);
      this.todos.unshift(x)   //把收到的数据添加在todos的前面
    },
    //勾选或者取消一个todo,
    //把此事件传给list,在传给item
    //由item给此函数传参
    checkedTodo(_,id) { //第一个参数为订阅消息的名字(使用下划线占位),第二个参数为订阅的消息传过来的数据
      this.todos.forEach((todo) => {  //todo是正在当前遍历的元素
        if (todo.id == id) todo.done = !todo.done
      })
    },
    //删除一个todo
    deletTodo(_,id) { 
      this.todos = this.todos.filter((todo) => {  //filter过滤掉不想要的 不改变数组 将新的数组赋值回去
        return todo.id !== id      //不等于id返回true
      })
    },
    //全选、全不选
    checkedAll(done){
      this.todos.forEach((todo)=>{
        todo.done = done
      })
    },
    //删除已完成
    cleartodo(){
      this.todos = this.todos.filter((todo)=>{ //通过过滤器把未选择的todo过滤出去 剩下的在赋值会todos
        return !todo.done == true
      })
    }
  },
  watch:{
    todos:{
      deep:true,
      handler(value){
      localStorage.setItem('todos',JSON.stringify(value))
    }
    }

    
  },
  //消息订阅
  mounted(){  //生命周期钩子,初始化页面完成时
    this.pubCheckedTodoId = pubsub.subscribe('checkedTodo',this.checkedTodo)  //pubsub.subscribe订阅消息,参数1:订阅消息名字,参数2:回调函数
    this.pubDelTodoId = pubsub.subscribe('deletTodo',this.deletTodo) //取消订阅需要:订阅消息的id,把id放在vc里,就可以读取到订阅消息的id
  },
  beforeDestroy(){  //在将要销毁时组件时,清除订阅的消息
    pubsub.unsubscribe(this.pubCheckedTodoId)
    pubsub.unsubscribe(this.pubDelTodoId)
  }
}
</script>

<style scoped>
/*base*/
body {
  background: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}

.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
