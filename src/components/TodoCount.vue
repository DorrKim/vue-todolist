<template>
  <div>
    전체 {{ countAllTodos }}개 중 {{ countCompletedTodos }}개를 완료했습니다.
  </div>
</template>

<script>
export default {
  props: {
    todos: {
      type: Array,
      default: function() {
        return []
      }
    }
  },
  computed: {
    countCompletedTodos() {
      return this.todos.filter(todo => todo.isCompleted).length
    },
    countAllTodos() {
      return this.todos.length
    }
  }
}
</script>