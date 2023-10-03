<template>
  <app-header />
  <main>
    <add-todo @AddNewTodo="AddTodos"></add-todo>
    <ul class="todos">
      <todo
        v-for="(item, i) in getTodo"
        :key="item.id"
        :todo="item"
        @dragstart="dragStart(i)"
        @Deleted="DeleteTodo"
        @changeStatus="changeTodoStatus"
        @dragover.prevent
        @drop="drop(i)"
      ></todo>
    </ul>
    <div class="card stat">
      <p class="corner">
        <span id="items-left">{{ getActiveTodoCount }}</span> مورد باقی مانده
      </p>
      <div class="filter">
        <button
          id="all"
          :class="{ on: activeTab == 'all' }"
          @click="changeTab('all')"
        >
          همه
        </button>
        <button
          id="active"
          :class="{ on: activeTab == 'active' }"
          @click="changeTab('active')"
        >
          فعال
        </button>
        <button
          id="completed"
          :class="{ on: activeTab == 'completed' }"
          @click="changeTab('completed')"
        >
          تکمیل
        </button>
      </div>
      <div class="corner">
        <button @click="deleteCompleted" id="clear-completed">
          حذف تکمیل شده ها
        </button>
      </div>
    </div>
  </main>
  <app-footer></app-footer>
</template>

<script setup>
import AppHeader from "./components/AppHeader.vue";
import AppFooter from "./components/AppFooter.vue";
import AddTodo from "./components/AddTodo.vue";
import Todo from "./components/Todo.vue";

const todos = ref([]);
const dragging = ref(-1);
const activeTab = ref("active");
const getActiveTodoCount = computed(() => {
  return todos.value.filter((f) => f.isComplete == false).length;
});
const getTodo = computed(() => {
  switch (activeTab.value) {
    case "all":
      return todos.value;
    case "active":
      return todos.value.filter((f) => f.isComplete == false);
    case "completed":
      return todos.value.filter((f) => f.isComplete == true);
    default:
      return todos.value;
  }
});
const AddTodos = (title) => {
  const id = Math.random().toString(16).slice(2);
  const todo = { id, title, isComplete: false };
  todos.value.push(todo);
};
const DeleteTodo = (id) => {
  todos.value = todos.value.filter((f) => f.id !== id);
};
const changeTodoStatus = (id, newStatus) => {
  var newTodos = [...todos.value];
  var selectedTodo = newTodos.find((f) => f.id === id);
  selectedTodo.isComplete = newStatus;
  todos.value = newTodos;
};
const deleteCompleted = () => {
  if (confirm("آیا از انجام عملیات اطمینان دارید ؟")) {
    var newTodos = [...todos.value];
    newTodos = newTodos.filter((f) => f.isComplete === false);
    todos.value = newTodos;
  }
};
const dragStart = (index) => {
  dragging.value = index;
};
const drop = (index) => {
  var newElement = todos.value.splice(dragging.value, 1)[0];
  todos.value.splice(index, 0, newElement);
};
const changeTab = (tab) => {
  activeTab.value = tab;
};
</script>

<style>
@import url("/css/styles.css");
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
