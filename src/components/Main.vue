<template>
  <section id="main" class="main" :data-theme="darkTheme? 'dark': 'light'">
    <img src="../assets/images/bg-desktop-dark.jpg" class="background-img">
    <div class="body">
      <section class="body-container">
        <div class="heading">
          <h1>TODO</h1>
          <button>
            <sun-icon v-if="darkTheme" @click="toggleTheme"></sun-icon>
            <MoonIcon v-else @click="toggleTheme"></MoonIcon>
          </button>
        </div>

        <div class="create container" @click="openTodoInput">
          <button @click="addNewTodoItem" class="checkbox"><check-icon class="check"></check-icon></button>
          <p v-if="!addTodo">Create a todo...</p>
          <input v-else @keyup.enter="addNewTodoItem" class="input" ref="todoItem">
        </div>
        <div class="todos container">
          <div class="list">
            <draggable v-if="showTodos || showAll" v-model="todoItems" group="todos">
              <div v-for="(todoItem, index) in todoItems" class="todo box">
                <button class="checkbox" @click="markCompleted(index)"></button>
                <p class="text">{{ todoItem }}</p>
              </div>
            </draggable>
            <draggable v-if="!showTodos || showAll" v-model="completedItems" group="completedTodos">
              <div v-for="(completedItem, index) in completedItems" class="completed box">
                <button class="checkbox" @click="unmarkCompleted(index)"><check-icon></check-icon></button>
                <p class="text">{{ completedItem }}</p>
              </div>
            </draggable>
          </div>
          <div class="footer box">
            <p>{{ todoItems.length }} Items left</p>
            <div class="footer-toes">
              <button @click="showAll = true" :style="showAll ? 'color: var(--bright-blue)' : ''">All</button>
              <button @click="showActive" :style="showTodos && !showAll ? 'color: var(--bright-blue)' : ''">Active</button>
              <button @click="showCompleted" :style="!showTodos && !showAll ? 'color: var(--bright-blue)' : ''">Completed</button>
            </div>
            <button @click="clearCompleted">Clear Completed</button>
          </div>
        </div>
      </section>
    </div>
  </section>
</template>

<script>

import { VueDraggableNext } from 'vue-draggable-next';

import CheckIcon from '../assets/images/icon-check.svg';
import SunIcon from '../assets/images/icon-sun.svg';
import MoonIcon from '../assets/images/icon-moon.svg';

export default {
  name: "Main",
  data() {
    return {
      todoItems: [],
      completedItems: [],
      showTodos: true,
      showAll: true,
      addTodo: false,
      darkTheme: true,
    }
  },
  mounted () {
    this.todoItems = JSON.parse(window.localStorage.getItem('todoItems')) ?? [];
    this.completedItems = JSON.parse(window.localStorage.getItem('completedItems')) ?? [];
  },
  components: {
    draggable: VueDraggableNext,
    CheckIcon,
    SunIcon,
    MoonIcon,
  },
  methods: {
    markCompleted(index) {
      this.completedItems.push(this.todoItems[index]);
      this.todoItems.splice(index, 1);
      this.updateLocalStorage();
    },
    unmarkCompleted(index) {
      this.todoItems.push(this.completedItems[index]);
      this.completedItems.splice(index, 1);
      this.updateLocalStorage();
    },
    clearCompleted() {
      this.completedItems = [];
      this.updateLocalStorage();
    },
    showActive() {
      this.showTodos = true;
      this.showAll = false;
    },
    showCompleted() {
      this.showAll = false;
      this.showTodos = false;
    },
    addNewTodoItem() {
      const item = this.$refs['todoItem'].value;
      if (item === '') return;

      this.todoItems.push(item);
      this.$refs['todoItem'].value = '';
      this.addTodo = false;
      this.updateLocalStorage();
    },
    updateLocalStorage() {
      window.localStorage.setItem('todoItems', JSON.stringify(this.todoItems));
      window.localStorage.setItem('completedItems', JSON.stringify(this.completedItems));
    },
    openTodoInput() {
      this.addTodo = true

      this.$nextTick(() => {
        this.$refs['todoItem'].focus()
      })
    },
    toggleTheme() {
      this.darkTheme = !this.darkTheme;
    }
  },
}
</script>