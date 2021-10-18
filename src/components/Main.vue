<template>
  <section id="main" class="main">
    <img src="../assets/images/bg-desktop-dark.jpg" class="background-img">
    <div class="body">
      <section class="body-container">
        <div class="heading">
          <h1>TODO</h1>
          <button><img src="../assets/images/icon-sun.svg"></button>
        </div>

        <div class="create">
          <h1>Create a todo...</h1>
        </div>
        <div class="todos">
          <div v-for="todoItem in todoItems" class="todo container">
            <button class="checkbox"><img src="../assets/images/icon-check.svg"></button>
            <p class="text">{{ todoItem }}</p>
          </div>
          <div class="footer container">
            <p>5 Items left</p>
            <div class="footer-toes">
              <button>All</button>
              <button>Active</button>
              <button>Completed</button>
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
      todoItems: ['Complete JavaScript course', 'Gor for 10 minute run', 'Drink a whole bottle of tequila'],
      completedItems: ['billy the bee']
    }
  },
  methods: {
    clearCompleted() {
      this.completedItems = [];
    }
  }
}
</script>

<style scoped>

</style>