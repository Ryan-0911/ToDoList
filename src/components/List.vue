<!-- eslint-disable vue/valid-template-root -->
<template>
  <ul class="todo-main">
    <TransitionGroup appear enter-active-class="animate__animated animate__backInLeft" leave-active-class="animate__animated animate__backOutRight">
      <Item v-for= "todoObj in todos" :key="todoObj.id" :todo="todoObj"></Item>
    </TransitionGroup>
  </ul>
</template>

<script>
import Item from "./Item.vue";
import 'animate.css';
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "List",
  components: { Item },
  props: ["todos",]
};
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
