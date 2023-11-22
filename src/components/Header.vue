<!-- eslint-disable vue/valid-template-root -->
<template>
    <div class="todo-header">
        <input type="text" placeholder="請輸入任務，按Enter確認" v-model="title" @keyup.enter="add"/>
    </div>
</template>

<script>
import {nanoid} from "nanoid"
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name: "Header",
    data(){
        return {
            title: ""
        }
    },
    methods: {
        add(){
            // 檢查輸入資料
            if(!this.title.trim()) return alert("輸入不得為空")
            // 將使用者的輸入包裝成一個todo物件
            const todoObj = {id:nanoid(), title:this.title, done: false}
            // 呼叫App組件的函式以添加一個todo物件
            this.$emit("addTodo", todoObj)
            // 清空輸入
            this.title = ""
        }
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
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
</style>