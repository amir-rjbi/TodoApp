<template>
  <div class="card add">
    <div class="cb-container">
      <button id="add-btn" @click="AddTodo">+</button>
    </div>
    <div class="txt-container">
      <label for="addt">افزودن </label>
      <input
        v-model="todoTitle"
        type="text"
        class="txt-input"
        placeholder="افزودن وظیفه جدید..."
        spellcheck="false"
        autocomplete="off"
        id="addt"
        dir="rtl"
        @keypress.enter="AddTodo()"
      />
    </div>
  </div>
</template>

<script setup>
const emit = defineEmits(["AddNewTodo"]);

const todoTitle = ref("");

const AddTodo = () => {
  emit("AddNewTodo", todoTitle.value);

  todoTitle.value = "";
};
</script>

<style></style>
