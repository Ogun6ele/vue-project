<script setup>
</script>

<template>
  <div class="todo-app">
    <h1>Todo Item</h1>

    <div class="input-container">
      <input
      v-model="newTodo"
      @keyup.enter="addTodo"
      placeholder="Enter new item"
    />

    <button
    @click="addTodo">Add</button>
    </div>
   

    <ul class="todo-list">
      <li v-for="todo in filteredTodos" :key="todo.id">
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
        <div class="todo-btn">
          <button @click="editTodo(todo)" class="edit-button">Edit</button>
          <button @click="deleteTodo(todo)" class="delete-button">Delete</button>
          
        </div>
        
      </li>
    </ul>

    <button @click="clearAllTodos">Clear All</button>
    <!-- <button @click="clearCompletedTodos">Clear Completed</button>

    <div>
      <label for="filter-status">Filter by status:</label>
      <select id="filter-status" v-model="filterStatus">
        <option value="all">All</option>
        <option value="done">Done</option>
        <option value="undone">Undone</option>
      </select>
    </div>

    <div>
      <label for="filter-text">Filter by text:</label>
      <input id="filter-text" type="text" v-model="filterText" />
    </div> -->
  </div>
</template>

<script>
import { ref, computed, watch } from "vue";

export default {
  setup() {
    const newTodo = ref("");
    const todos = ref([]);
    const filterStatus = ref("all");
    const filterText = ref("");

    const filteredTodos = computed(() => {
      return todos.value.filter((todo) => {
        const isFilteredByStatus =
          filterStatus.value === "all" || filterStatus.value === todo.done;
        const isFilteredByText =
          !filterText.value || todo.text.includes(filterText.value);
        return isFilteredByStatus && isFilteredByText;
      });
    });

    const addTodo = () => {
      if (!newTodo.value.trim()) return;
      todos.value.push({
        id: Date.now(),
        text: newTodo.value.trim(),
        done: false,
      });
      newTodo.value = "";
    };

    const deleteTodo = (todo) => {
      const index = todos.value.indexOf(todo);
      if (index !== -1) {
        todos.value.splice(index, 1);
      }
    };

    const editTodo = (todo) => {
      const newText = prompt("Enter new todo text:", todo.text);
      if (newText) {
        todo.text = newText;
      }
    };

    




    const toggleTodoStatus = (todo) => {
      todo.done = !todo.done;
    };

    const clearAllTodos = () => {
      todos.value = [];
    };

    const clearCompletedTodos = () => {
      todos.value = todos.value.filter((todo) => !todo.done);
    };

    watch(
      todos,
      () => {
        localStorage.setItem("todos", JSON.stringify(todos.value));
      },
      { deep: true }
    );

    const storedTodos = localStorage.getItem("todos");
    if (storedTodos) {
      todos.value = JSON.parse(storedTodos);
    }

    return {
      newTodo,
      todos,
      filterStatus,
      filterText,
      filteredTodos,
      addTodo,
      deleteTodo,
      editTodo,
      toggleTodoStatus,
      clearAllTodos,
      clearCompletedTodos,
    };
  },
};
</script>

<style scoped>
  body{
    background-color: #000;
    color: #fff
  }
</style>
