<template>
  <div id="todo-app">
    <h1>{{ title }}</h1>
    <todo-input @onSubmit="addNewTodo" />
    <todo-list
      :todos="todos"
      @onClickDeleteBtn="deleteTodo"
      @onToggle="toggleTodo"
      @onEdit="editTodo" />
    <todo-count :todos="todos" />
  </div>
</template>

<script>
import TodoInput from '~/components/TodoInput'
import TodoList from '~/components/TodoList'
import TodoCount from '~/components/TodoCount'

export default {
  components: {
    TodoInput,
    TodoList,
    TodoCount
  },
  data() {
    return {
      title: 'Hyunseok\'s Todo List',
      todos: []
    }
  },
  created() {
    try {
      const storedItem = localStorage.getItem('todos') || []
      this.todos = JSON.parse(storedItem)
    } catch(e) {
      console.error('저장된 값을 불러오지 못했습니다.')
    }
  },
  methods: {
    addNewTodo(newTodoText) {
      if (!newTodoText) return
      this.todos.push({
        id: Date.now(),
        text: newTodoText,
        isCompleted: false
      })
      this.saveTodos()
    },
    deleteTodo(selectedId){
      const newTodos = [...this.todos]
      this.todos = newTodos.filter(todo => todo.id !== selectedId)
      this.saveTodos()
    },
    toggleTodo(selectedId){
      const selectedTodo = this.todos.find(todo => todo.id === selectedId)
      selectedTodo.isCompleted = !selectedTodo.isCompleted
      this.saveTodos()
    },
    editTodo(newTodoText, selectedId) {
      const selectedTodo = this.todos.find(todo => todo.id === selectedId)
      selectedTodo.text = newTodoText
      this.saveTodos()
    }
    ,
    saveTodos() {
      const parsedTodos = JSON.stringify(this.todos)
      localStorage.setItem('todos', parsedTodos)
    }
  }
}
</script>

