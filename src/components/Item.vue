<!-- eslint-disable vue/valid-template-root -->
<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.done"
        @change="handleCheck(todo.id)"
      />
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <input ref="inputTitle" v-show="todo.isEdit" type="text" :value="todo.title" @blur="handleBlur(todo, $event)">
    </label>
    <button class="btn btn-danger" @click="handleDelete(todo.id)">刪除</button>
    <button class="btn btn-edit" v-show="!todo.isEdit" @click="handleEdit(todo)">編輯</button>

  </li>
</template>

<script>
import pubsub from "pubsub-js"
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Item",
  props: ["todo"],
  methods: {
    handleCheck(id) {
      this.$bus.$emit("checkTodo",id);
    },
    handleDelete(id) {
      if (confirm("確定刪除嗎?")) {
        pubsub.publish("deleteTodo", id);
      }
    },
    handleEdit(todo){
      // eslint-disable-next-line no-prototype-builtins
      if (todo.hasOwnProperty("isEdit"))
      {
        todo.isEdit = true
      }
      else
      {
        console.log("@")
        this.$set(todo, "isEdit", true)
      }
      this.$nextTick(function(){
        this.$refs.inputTitle.focus()
      })
    },
    // 失去焦點的回調 (真正執行修改邏輯)
    handleBlur(todo, e){
      todo.isEdit = false
      if (!e.target.value.trim()) return alert("輸入不得為空!")
      this.$bus.$emit("updateTodo", todo.id, e.target.value)
    }
  },
};
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
  background-color: #ddd;
}

li:hover button {
  display: block;
}
</style>
