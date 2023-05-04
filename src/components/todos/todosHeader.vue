<template>
  <div>
    <h2>TODO LIST</h2>
    <el-input
      v-model="input"
      @keyup.enter="add"
      placeholder="請輸入待辦事項..."
      clearable
    />
  </div>
</template>

<script setup lang="ts">
import { ref, defineEmits } from "vue";
import { nanoid } from "nanoid";

const emit = defineEmits(["addTodo"]);
const input = ref("");

// 透過v-model取值：
// (1)校驗輸入值 (2)將用戶的輸入包裝成todoObj對象 (3)將todoObj傳給父組件並清除資料
const add = () => {
  if (!input.value.trim()) return alert("輸入不能為空");
  const todoObj = {
    id: nanoid(),
    title: input.value,
    isChecked: false,
  };
  emit("addTodo", todoObj);
  input.value = "";
};
</script>

<style scoped></style>
