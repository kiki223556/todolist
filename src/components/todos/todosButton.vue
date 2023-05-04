<template>
  <div class="button-container">
    <el-button @click="centerDialogVisible = true" class="todos-button">
      todosList Button
    </el-button>

    <el-dialog v-model="centerDialogVisible" width="600px">
      <template #header>
        <todos-header v-on:addTodo="addTodo" />
        <todos-list :todosArr="todosArr" />
      </template>
      <template #footer>
        <todos-footer
          :todosArr="todosArr"
          v-on:checkAllTodo="checkAllTodo"
          v-on:clearDoneTodo="clearDoneTodo"
        />
      </template>
    </el-dialog>
  </div>
</template>

<script lang="ts" setup>
import { ref, reactive, watch } from "vue";
import { onUnmounted, onMounted } from "vue";
import { emitter } from "@/mitt/mitter";
import TodosHeader from "@/components/todos/todosHeader.vue";
import TodosList from "@/components/todos/todosList.vue";
import TodosFooter from "@/components/todos/todosFooter.vue";

const centerDialogVisible = ref(false);

// 1.將數組傳給todos-list，寫死了，沒有辦法讀取localStorage資料
// const todosArr = reactive([
//   { id: "1", title: "sing", isChecked: false },
//   { id: "2", title: "eating", isChecked: true },
//   { id: "3", title: "dancing", isChecked: false },
// ]);

// 2.初始為空數組，後續從localStorage讀取
const todosArr =
  reactive(JSON.parse(localStorage.getItem("todosArray"))) || reactive([]);

// 將函數傳給todos-header，並添加一個todoObj
const addTodo = (todoObj) => {
  todosArr.unshift(todoObj);
};

// 取消或勾選一個todo
const checkTodo = (id) => {
  todosArr.forEach((todo) => {
    if (todo.id === id) todo.isChecked = !todo.isChecked;
  });
};

// 刪除一個todo
const deleteTodo = (id) => {
  const index = todosArr.findIndex((todo) => todo.id === id);
  todosArr.splice(index, 1);
};

// 全選或全不選todo
const checkAllTodo = (isChecked) => {
  todosArr.forEach((todo) => {
    todo.isChecked = isChecked;
  });
};

// 清除所有已完成的todo
const clearDoneTodo = () => {
  todosArr.splice(
    0,
    todosArr.length,
    ...todosArr.filter((todo) => !todo.isChecked)
  );
};

// 更新todo內容
const updateTodo = (blurEvent) => {
  todosArr.forEach((todo) => {
    if (todo.id === blurEvent.id) todo.title = blurEvent.title;
  });
};

watch(
  () => todosArr,
  (value) => {
    localStorage.setItem("todosArray", JSON.stringify(value));
  },
  { deep: true }
);

// 用emitter實現子傳父id進行刪除及勾選函數
// 1.在"@/mitt/mitter" export {emitter}，並引入父及子組建
// 2.onMounted：註冊一個監聽器函數emitter.on(eventName, listener)
// 3.onUnmounted：取消一個監聽器函數emitter.off(eventName, listener)
// 4.在子組建觸發一個事件，並傳遞相應的參數：emitter.emit(eventName, ...args)
// 5.實現跨組件的事件通信，並且可以避免直接修改父組件的狀態造成的代碼耦合性問題
onMounted(() => {
  emitter.on("checkTodo", checkTodo);
  emitter.on("updateTodo", updateTodo);
  emitter.on("deleteTodo", deleteTodo);
});

onUnmounted(() => {
  emitter.off("checkTodo", checkTodo);
  emitter.off("updateTodo");
  emitter.off("deleteTodo", deleteTodo);
});
</script>

<style scoped>
.dialog-footer button:first-child {
  margin-right: 10px;
}
.el-button,
.todos-button {
  padding: 30px;
}

.button-container {
  /* // height: 100%;
  // width: 100%; */
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
