<template>
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput @add:todo="addTodo"></TodoInput>
    <TodoList :todo-arr="todoItems" @remove:todo="removeTodo" @toggle:todo="toggleComplete"></TodoList>
    <TodoFooter @clear:todo="clearTodo"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from '@/components/TodoHeader'
import TodoInput from '@/components/TodoInput.vue'
import TodoList from '@/components/TodoList.vue'
import TodoFooter from '@/components/TodoFooter.vue'
import { onBeforeMount, reactive } from 'vue'

export default {
  name: 'App',
  components: {
    TodoHeader,
    TodoInput,
    TodoList,
    TodoFooter
  },
  setup () {
    const todoItems = reactive([])

    onBeforeMount(() => {
      if (localStorage.length > 0) {
        for (var i = 0; i < localStorage.length; i++) {
          const itemJson = localStorage.getItem(localStorage.key(i))
          if (itemJson) {
            todoItems.push(JSON.parse(itemJson))
          } // if
        } // for
      } // if
    })

    const addTodo = (todoItemStr) => {
      const todoItemObj = { completed: false, item: todoItemStr }
      localStorage.setItem(todoItemStr, JSON.stringify(todoItemObj))
      todoItems.push(todoItemObj)
    } // addTodo

    const removeTodo = (todoItemStr, index) => {
      localStorage.removeItem(todoItemStr)
      todoItems.splice(index, 1)
    }

    const toggleComplete = (todoItem, index) => {
      const { completed, item } = todoItem
      todoItem[index].completed = !completed
      localStorage.removeItem(item)
      localStorage.setItem(item, JSON.stringify(todoItem[index]))
    }

    const clearTodo = () => {
      localStorage.clear()
      todoItems.splice(0)
    }

    return { todoItems, addTodo, removeTodo, toggleComplete, clearTodo }
  } // setup

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  text-align: center;
  background-color: #f6f6f6;
}

input {
  border-style: groove;
  width: 200px;
}

button {
  border-style: groove;
}

.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>
