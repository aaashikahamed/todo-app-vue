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

  }

  // this function will watch 'name', and if it's changes we will get 'newVal', we will set it to 'localStorage' name 
  watch(name,(newVal) => {
    localStorage.setItem('name', newVal)
  })

  // this function will get 'name value' from local storage if there is any and will assign it to name value
  onMounted(() => {
    name.value = localStorage.getItem('name') || ''
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
          <label for="">
            <input type="radio" name="category" value="business" v-model="input_category">
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label for="">
            <input type="radio" name="category" value="business" v-model="input_category">
            <span class="bubble business"></span>
            <div>Business</div>
          </label>
        </div>
      </form>
     </section>
  </main>


</template>
