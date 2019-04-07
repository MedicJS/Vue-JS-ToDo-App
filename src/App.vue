<template>
  <div id="app" class='container'>
    <h1 class='text-center'>To-Do App in Vue.JS </h1>
    <hr>
    <form @submit.prevent='addTodo'>
      <input v-model="newTodo"  type='text' aria-describedby='newTodoHelp' placeholder='Add New To-Do...'>
      <button type='submit' id='add' class='btn btn-primary'>Add</button>
      <small id='newTodoHelp' class='form-text text-muted'>Enter a new To-Do.</small>
    </form>
    <div style="text-align: right">
    <button class='btn btn-danger reset' @click='resetTodos'>Reset List</button>
    </div>
    <ul class="list-group">
      <li v-for='(todo, i) in todos' class="list-group-item d-flex justify-content-between align-items-center">
        <small style='float: right;'>{{todo.date}}</small>
        <span :class="{isDone: todo.done}">{{todo.title}}</span>
        <div>
          <button v-if='!todo.done' @click='markDone(todo)' type='button' class='btn btn-success btn-sm'><i class="material-icons">
          check
          </i></button>
          <button @click='removeTodo(i)' class='btn btn-danger btn-sm delete-btn'><i class="material-icons">
          X
          </i></button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      newTodo: '',
      //Todo List Array of Objects
      todos: []
    }
  },
  watch: {
    todos: {
      handler() {
        localStorage.todos = JSON.stringify(this.todos);
      },
      deep: true
    }
  },
  mounted() {
    if(localStorage.todos){
      this.todos = JSON.parse(localStorage.todos);
    }
  },
  methods: {
    addTodo() {
      //Start new date
      let date = new Date()
      //prevent empty todo being created
      if(this.newTodo != ''){
      //Create a Todo Object and Push into List Array
      this.todos.push({
        title: this.newTodo,
        done: false,
        date: date.getMonth() + '/' + date.getDate() + '/' + date.getFullYear()
      });
      }
      //Clears the Input after submission
      this.newTodo = ''
    },
    //Check Function
    markDone(todo) {
      todo.done = true
    },
    removeTodo(todo) {
      this.todos.splice(todo, 1);
    },
    resetTodos(){
      this.todos = [];
    }
  }
}
</script>

<style>
  #app {
    width: 82%;
    max-width: 500px;
  }
  
  h1 {
    margin-top: 10px;
  }

  input {
    margin-top: 10px;
    border: 2px solid black;
    padding: 8px;
    font-size: 18px;
    font-style: bold;
    display: inline;
    width: 90%;
    box-shadow: 1px 1px 1px 1px grey;
    z-index: 0;
  }
  
  #add {
    margin-top: 11px;
    display: inline;
    float: right;  
    z-index: 2;
    position: absolute;
    
  }

  .reset {
    margin-top: 8px;
    box-shadow: 1px 1px 1px 1px black;
    z-index: 2;
  }

  .list-group {
    margin-top: 30px;
    -webkit-box-shadow: 9px 10px 30px -4px rgba(15,15,15,1);
    -moz-box-shadow: 9px 10px 30px -4px rgba(15,15,15,1);
    box-shadow: 5px 8px 20px -10px rgba(15,15,15,1);
  }

  button {
    
    text-shadow: 2px 2px 2px black;
    display: inline-block;
    box-shadow: 1px 1px 1px 1px black;
  }

  ul {
    margin-top: 15px;
  }


  .material-icons {
    font-size: 15px;
    width: 11px;
    height: 10px;
  }

  .isDone {
    text-decoration: line-through;
  }

  
  footer {
    margin-top: 100px;
  }



  
</style>
