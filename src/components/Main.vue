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
          <div class="todo container">
            <button class="checkbox"><img src="../assets/images/icon-check.svg"></button>
            <p class="text">Todo</p>
          </div>
          <hr>
          <div class="footer container">
            <p>5 Items left</p>
            <div class="display-select">
              <button>All</button>
              <button>Active</button>
              <button>Completed</button>
            </div>
            <p>Clear completed</p>
          </div>
        </div>
      </section>
    </div>
  </section>
</template>

<script>
export default {
  name: "Main"
}
</script>

<style scoped>

</style>