<script setup>
import {ref, onMounted, computed, watch} from "vue"

const todos = ref([]);
const username = ref("");
const todo_input = ref("");
const input_category = ref(null)

const todos_ascending = computed(() => todos.value.sort((a,b) => {
    return b.date_created - a.date_created;
  }
))

const addTodo = () => {
  if(todo_input.value === "" || input_category.input_category === null) {
    return
  }

todos.value.push({
  content: todo_input.value,
  category: input_category.value,
  done: false,
  date_created: new Date().getTime()
})
  
}

watch(todos, (todosNewValue) => {
  localStorage.setItem("todos", JSON.stringify(todosNewValue))
}, {deep: true})

watch(username, (nameNewValue) => {
  localStorage.setItem("username", nameNewValue)
})

onMounted(() => {
  username.value = localStorage.getItem("username") || ""
  todos.value = JSON.parse(localStorage.getItem("todos")) || []
})


</script>

<template>

  <main>

    <section>
      <h3>Hi there,
        <input class="name-input" type="text" v-model="username" placeholder="guest" />
      </h3>
    </section>

    <section>
      <h3>WHAT YOU HAVE TO ACHIEVE</h3>

      <form @submit.prevent="addTodo">
        <input 
          class="todo-input" 
          type="text" 
          placeholder="enter a task" 
          v-model="todo_input">

        <h4>PICK A CATEGORY</h4>

        <div class="categories">

          <label for="category">
            <input 
              type="radio" 
              name="category" 
              value="business" 
              v-model="input_category">
            <span>business</span>
          </label>

          <label for="category">
            <input 
              type="radio" 
              name="category" 
              value="leisure" 
              v-model="input_category">
            <span>leisure</span>

          </label>

          <input type="submit" value="add a todo" />

          <section>
            <div>{{todos_ascending}}</div>
          </section>

        </div>

      </form>

    </section>

  </main>

</template>

