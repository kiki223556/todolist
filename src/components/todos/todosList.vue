<template>
  <ul class="todos-main">
    <todosItem v-for="todo in todosArr" :key="todo.id" :todo="todo" />
  </ul>
</template>

<script setup lang="ts">
import { defineProps } from "vue";
import todosItem from "@/components/todos/todosItem.vue";

// const props = defineProps(["todosArr", "checkTodo", "deleteTodo"]);
const props = defineProps(["todosArr"]);

console.log("todosArr", props.todosArr);
</script>

<style scoped>
.todos-main {
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 16px;
  margin: 18px 0px 18px 0px;
  padding: 0px;
}

.todos-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding-left: 5px;
  margin-top: 10px;
}
.list-container {
  display: flex;
  justify-content: flex-start;
  align-items: center;
}

.item {
  font-size: 16px;
  margin: 18px 0px 18px 10px;
  padding: 3px;
  width: 100%;
  border: 1px solid #bbb7b7;
  border-radius: 4px;
}
</style>
