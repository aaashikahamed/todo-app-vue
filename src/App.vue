<script setup>
  // these things we can use with composition API
  //  ref - How we are gonna handle the states
  // onMounted - going to be used when we render the page or component. this will used to call our storage out / use storage
  // computed - used to display the items in time order
  // watch - watch  for any changes in our ref (references) and update in storage
  import {ref, onMounted, computed, watch} from 'vue'

  const todos = ref([])
  const name = ref('')

  // contents we will add into input field
  const input_content = ref('')

  const input_category = ref(null)

  const todos_asc = computed(() => todos.value.sort((a,b) => {
    return b.createdAt - a.createdAt
  }))

  const addTodo = () =>{
    if(input_content.value.trim() === '' || input_category.value === null){
      return
    }

    todos.value.push({
      content : input_content.value,
      category : input_category.value,
      done: false,
      createdAt: new Date().getTime()
    })

    input_content.value = ''
    input_category.value = null

  }

  const removeTodo = todo => {
    todos.value = todos.value.filter(t => t !== todo)
  }

  // this will only work when we set our 'todos' for first time
  watch (todos, (newVal) => {
    localStorage.setItem('todos', JSON.stringify(newVal))
  }), {deep: true} // "deep" will look through everything, it will loog throygh each individual array item, if anything changes inside, deep will catch it



  // this function will watch 'name', and if it's changes we will get 'newVal', we will set it to 'localStorage' name 
  watch(name,(newVal) => {
    localStorage.setItem('name', newVal)
  })

  // this function will get 'name value' from local storage if there is any and will assign it to name value
  onMounted(() => {
    name.value = localStorage.getItem('name') || ''
    todos.value = JSON.parse(localStorage.getItem('todos')) || []
  })
</script>

<template>
  
  <main class="app">
     <section class="greeting">
      <h2 class="title">
        what's up, <input type="text" placeholder="Name here" v-model="name">
      </h2>
     </section>

     <section class="create-todo">
      <h3>CREATE A TODO</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input type="text" placeholder="e.g. make a video" v-model="input_content">

        <h4>Pick a Category</h4>
        
        <div class="options">
          <label >
            <input type="radio" name="category" value="business" v-model="input_category">
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input type="radio" name="category" value="personal" v-model="input_category">
            <span class="bubble personal"></span>
            <div>personal</div>
          </label>
        </div>

        <input type="submit" value="Add todo">

      </form>
     </section>

     <section class="todo-list">
      <h3>TODO LIST</h3>
      <div class="list">
        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
          <label >
            <input type="checkbox" name=""  v-model="todo.done" id="">
            <span :class="`bubble ${todo.category}`"></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todo.content">
          </div>
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
     </section>
  </main>


</template>
