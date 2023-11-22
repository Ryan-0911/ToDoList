<!-- eslint-disable vue/valid-template-root -->
<template>
  <div class="todo-footer" v-show="total">
    <label>
      <!-- <input type="checkbox" :checked="isAll" @change="checkAll"/> -->
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成{{ doneTotal }}</span> / 全部{{ total }}
    </span>
    <button class="btn btn-danger" @click="clearDone">清除已完成任務</button>
  </div>
</template>

<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Footer",
  props: ["todos"],
  computed: {
    doneTotal() {
      return this.todos.reduce(
        (done, todo) => (done += todo.done == true ? 1 : 0),
        0
      );
    },
    total() {
      return this.todos.length;
    },
    isAll: {
      get() {
        return this.total == this.doneTotal && this.todos > 0;
      },
      set(value) {
        this.$emit("checkAllTodo", value);
      },
    },
  },
  methods: {
    clearDone() {
      this.$emit("clearDoneTodo");
    },
  },
};
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
