<template>
  <h2>todolist功能展示</h2>
  <TodosButton></TodosButton>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import TodosButton from "./components/todos/todosButton.vue";

export default defineComponent({
  name: "App",
  components: {
    TodosButton,
  },
});
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
</style>
