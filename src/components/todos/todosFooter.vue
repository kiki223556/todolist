<template>
  <div class="todos-footer" v-show="total">
    <label>
      <!-- <input type="checkbox" :checked="isAll" @change="checkAll" /> -->
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成{{ doneTotal }}</span> / 全部{{ total }}
    </span>
    <el-button type="danger" @click="clearAll">清除已完成任務</el-button>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits, computed } from "vue";

// 傳數組作為computed計算，用props(父傳子)
const props = defineProps(["todosArr"]);
// 傳函數方法，可用emit(子傳父)
const emit = defineEmits(["checkAllTodo", "clearDoneTodo"]);

const total = computed(() => {
  return props.todosArr.length;
});

// 計算已完成的任務數量
// 方法1: forEach
// const doneTotal = computed(() => {
//   let i = 0;
//   todosArr.forEach((todo) => {
//     if (todo.isChecked) i++;
//   });
//   return i;
// });

// 方法2: reduce
const doneTotal = computed(() => {
  return props.todosArr.reduce(
    (pre, todo) => pre + (todo.isChecked ? 1 : 0),
    0
  );
});

// 合併定義初始值是否全選，及全選方法
const isAll = computed({
  get() {
    return total.value === doneTotal.value && total.value > 0;
  },
  set(value) {
    // props.checkAllTodo(value); // 原使用props寫法
    emit("checkAllTodo", value);
  },
});

// 分別定義初始值是否全選，及全選方法
// const isAll = computed(() => {
//   return total.value === doneTotal.value && total.value > 0;
// });

// const checkAll = (e) => {
//   props.checkAllTodo(e.target.checked);
// };

const clearAll = () => {
  // props.clearDoneTodo();
  emit("clearDoneTodo");
};
</script>

<style scoped>
.todos-footer {
  height: 40px;
  line-height: 40px;
  padding-right: 20px;
  padding-left: 15px;
  margin-top: 5px;
  text-align: left;
}

.todos-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todos-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todos-footer button {
  float: right;
  margin-top: 5px;
}
</style>
