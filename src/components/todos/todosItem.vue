<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.isChecked"
        @change="handleCheck(todo.id)"
      />
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <input
        type="text"
        ref="inputTitle"
        v-show="todo.isEdit"
        v-model="title"
        @blur="handleBlur(todo)"
      />
    </label>
    <el-button
      plain
      circle
      class="itemBtn"
      type="danger"
      @click="handleDelete(todo.id)"
    >
      <el-icon><Delete /></el-icon>
    </el-button>
    <el-button plain circle class="itemBtn" @click="handleEdit(todo)">
      <el-icon><Edit /></el-icon>
    </el-button>
  </li>
</template>

<script setup lang="ts">
import { defineProps, ref, nextTick } from "vue";
import { emitter } from "@/mitt/mitter";

const props = defineProps(["todo"]);
// const props = defineProps<{ todo: any[] }>();

const title = ref(props.todo.title);
const inputTitle = ref();

const handleCheck = (id) => {
  // props.checkTodo(id); // 父傳子props寫法
  emitter.emit("checkTodo", id); // 子傳父emitter寫法
};

// 刪除
const handleDelete = (id) => {
  if (confirm("確定刪除嗎？")) {
    // props.deleteTodo(id);
    emitter.emit("deleteTodo", id);
  }
};

// 編輯
const handleEdit = async (todo) => {
  todo.isEdit = true;
  // 用nextTick執行異步函數，等下一次dom節點更新後再執行focus()
  await nextTick(() => {
    inputTitle.value.focus();
  });

  // setTimeout(() => {
  //   inputTitle.value.focus();
  // }, 200);

  // vue3可直接添加屬性，應該不用判斷裡面是否有無isEdit屬性吧？
  // if ("isEdit" in todo) {
  //   console.log("1");
  //   todo.isEdit = true;
  // } else {
  //   console.log("2");
  //   todo.isEdit = true;
  // }
};

// 失去焦點回調（真正執行修改邏輯）
// const handleBlur = (todo, e) => {
//   todo.isEdit = false;
//   emitter.emit("updateTodo", (todo.id, e.target.value)); // 子傳父emitter寫法
// };
const handleBlur = (todo) => {
  todo.isEdit = false;
  const blurEvent = {
    ...todo,
    title,
  };
  emitter.emit("updateTodo", blurEvent);
  console.log("updateTodo", blurEvent.title);
};
</script>

<style scoped>
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
  margin-left: 6px;
  position: relative;
}

li .itemBtn {
  float: right;
  display: none;
  align-items: center;
  margin: 2px;
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

li:hover .itemBtn {
  display: block;
}
</style>
