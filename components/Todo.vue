<template>
  <li class="card" draggable="true">
    <div class="cb-container">
      <input
        type="checkbox"
        :checked="todo.isComplete ? true : null"
        class="cb-input"
        @click="changeStatus"
      />
      <span class="check"></span>
    </div>
    <p class="item">
      <del v-if="todo.isComplete == true">{{ todo.title }}</del>
      <span v-else v-text="todo.title"></span>
    </p>
    <button class="clear" @click="deleteTodo">
      <img :src="crossImage" alt="Clear it" />
    </button>
  </li>
</template>

<script setup>
import Cross from "../assets/icon-cross.svg";

const props = defineProps({
  todo: Object,
});
const crossImage = Cross;
const emit = defineEmits(["Deleted", "changeStatus"]);
const deleteTodo = () => {
  if (confirm("آیا از حذف اطمینان دارید ؟")) {
    emit("Deleted", props.todo.id);
  }
};
const changeStatus = () => {
  emit("changeStatus", props.todo.id, !props.todo.isComplete);
};
</script>

<style></style>
