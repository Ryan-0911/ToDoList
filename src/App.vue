<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <Header @addTodo="addTodo"></Header>
        <List :todos="todos"></List>
        <Footer :todos="todos" @checkAllTodo="checkAllTodo" @clearDoneTodo="clearDoneTodo"></Footer>
      </div>
    </div>
  </div>
</template>

<script>
import pubsub from "pubsub-js"
import Header from "./components/Header.vue";
import List from "./components/List.vue";
import Footer from "./components/Footer.vue";

export default {
  name: "App",
  components: {
    // eslint-disable-next-line vue/no-unused-components
    Header,
    // eslint-disable-next-line vue/no-unused-components
    List,
    // eslint-disable-next-line vue/no-unused-components
    Footer,
  },
  data(){
    return {
      todos:JSON.parse(localStorage.getItem("todos")) || []
    }
  },
  methods:{
    // 添加一個todo物件
    addTodo(todoObj){
      this.todos.unshift(todoObj);
    },
    //勾選或取消勾選todo物件
    checkTodo(id){
      this.todos.forEach((todo) =>{
        if (todo.id == id){
          todo.done = !todo.done
        }
      })
    },
    // 刪除一個todo物件
    deleteTodo(_,id){
      this.todos = this.todos.filter(todo => todo.id != id)
    },
    // 更新todo物件標題
    updateTodo(id, title){
      this.todos.forEach((todo) =>{
        if (todo.id == id){
          todo.title = title
        }
      })
    },
    // 全選或全部選
    checkAllTodo(check){
      this.todos.forEach((todo) => todo.done = check)
    },
    // 清除已完成的todo
    clearDoneTodo(){
      this.todos = this.todos.filter(todo => !todo.done)
    }
  },
  watch: {
    todos:{
      deep: true,
      handler(value){
        localStorage.setItem("todos", JSON.stringify(value))
      }
    }
  },
  mounted(){
    this.$bus.$on("checkTodo", this.checkTodo)
    this.$bus.$on("updateTodo", this.updateTodo)
    this.pid = pubsub.subscribe("deleteTodo", this.deleteTodo)
  },
  beforeDestroy(){
    this.$bus.$off("checkTodo")
    this.$bus.$off("updateTodo")
    pubsub.unsubsribe(this.pid)
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
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
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-edit {
  color: #fff;
  background-color: skyblue;
  border: 1px solid rgb(75, 125, 144);
  margin-right: 5px;
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
