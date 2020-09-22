<template>
  <input v-model="todoState.input">
  <button @click="addTodo">add todo</button>

  <div>todos</div>
  <ul>
    <li v-for="{value, id} in todoState.todos" :key="id">{{value}}<button @click="deleteTodo(id)">X</button><button @click="doneTodo(id)">O</button></li>
  </ul>
  <div>done</div>
  <ul>
    <li v-for="{value, id} in todoState.doneTodos" :key="id">{{value}}<button @click="notDoneTodo(id)">X</button></li>
  </ul>
</template>

<script>
import {computed, reactive} from 'vue'

export default {
  setup() {
    const [todoState] = useTodo()
    const [addTodo, deleteTodo, doneTodo, notDoneTodo] = setTodo(todoState)
    return {
      todoState,
      addTodo,
      deleteTodo,
      doneTodo,
      notDoneTodo
    }
  }
}

function useTodo(){
  const todoState = reactive({
    todos: [{value: 'sample', id: 0}],
    input: '',
    id: 1,
    doneTodos: []
  })
  return [todoState]
}

function setTodo(todoState){
  function addTodo(){
    todoState.id ++
    todoState.todos.push({value: todoState.input, id: todoState.id})
    todoState.input = ''
  }

  function deleteTodo(id){
    todoState.todos = todoState.todos.filter(todo=>todo.id !== id)
  }

  function doneTodo(id){
    todoState.doneTodos = todoState.todos.filter(todo=>todo.id === id)
    todoState.todos = todoState.todos.filter(todo=>todo.id !== id)
  }

  function notDoneTodo(id){
    todoState.todos = todoState.doneTodos.filter(todo=>todo.id === id)
    todoState.doneTodos = todoState.doneTodos.filter(todo=>todo.id !== id)
  }
  return [addTodo, deleteTodo, doneTodo, notDoneTodo]
}
</script>
