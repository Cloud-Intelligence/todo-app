<template>
  <section id="main" class="main">
    <img src="../assets/images/bg-desktop-dark.jpg" class="background-img">
    <div class="body">
      <section class="body-container">
        <div class="heading">
          <h1>TODO</h1>
          <button><img src="../assets/images/icon-sun.svg"></button>
        </div>

        <div class="create container" @click="openTodoInput">
          <button @click="addNewTodoItem" class="checkbox"><img src="../assets/images/icon-check.svg"></button>
          <h1 v-if="!addTodo">Create a todo...</h1>
          <input v-else @keyup.enter="addNewTodoItem" class="input" ref="todoItem">
        </div>
        <div class="todos">
          <div v-if="showTodos || showAll" v-for="(todoItem, index) in todoItems" class="todo container">
            <button class="checkbox" @click="markCompleted(index)"></button>
            <p class="text">{{ todoItem }}</p>
          </div>
          <div v-if="!showTodos || showAll" v-for="(completedItem, index) in completedItems" class="completed container">
            <button class="checkbox" @click="unmarkCompleted(index)"><img src="../assets/images/icon-check.svg"></button>
            <p class="text">{{ completedItem }}</p>
          </div>
          <div class="footer container">
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
export default {
  name: "Main",
  data() {
    return {
      todoItems: [],
      completedItems: [],
      showTodos: true,
      showAll: false,
      addTodo: false,
    }
  },
  mounted() {
    this.todoItems = JSON.parse(window.localStorage.getItem('todoItems')) ?? [];
    this.completedItems = JSON.parse(window.localStorage.getItem('completedItems')) ?? [];
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
  }
}
</script>

<style scoped>

</style>