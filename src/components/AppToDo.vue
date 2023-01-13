<template>
    <div class="body">
      <div class="card-todo">
        <h3 class="title" >Bem vindo ao todo list!</h3>
        <TodoList/>
      </div>
    </div>
</template>

<script  lang="ts">
import TodoList from './TodoList.vue'

  export default {
    components: { TodoList },
    data () {
      return {
      }
    },
  }
</script>

<style scoped>

  .body{
    display: flex;
    justify-content:center;
    align-items: center;
    height: 100%;
    background-color: #F0FFF0
  }

  .card-todo {
    display: flex;
    flex-direction: column;
    justify-content:center;
    align-items: center;
    padding: 1rem;
    margin: 7rem;
    border: 1px solid #696969;
    border-radius: 8px;
    background-color: #FFFFFF;

  }

</style>
