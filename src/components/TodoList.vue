<template>
  <ul>
    <li
      v-for="todo in todos"
      :key="todo.id">
      <input
        type="checkbox"
        :checked="todo.isCompleted" 
        @change="$emit('onToggle', todo.id)" />
      
      <label
        v-show="todo.id !== selectedId"
        :class="{completed: todo.isCompleted}"
        @dblclick="openEditor($event, todo.id)">{{ todo.text }}</label>
      
      <input
        v-show="todo.id === selectedId"
        ref="edit"
        @keydown.esc.enter="editTodoText($event, todo.id)" />
      <button @click="$emit('onClickDeleteBtn', todo.id)">
        X
      </button>
    </li>
  </ul>
</template>

<script>
import { nextTick } from '@vue/runtime-core'
export default {
  props: {
    todos: {
      type: Array,
      default: function() {
        return []
      }
    }
  },
  emits: ['onToggle', 'onClickDeleteBtn', 'onEdit'],
  data() {
    return{
      selectedId: '',
      editedText: ''
    }
  },
  methods: {
    openEditor(event, selectedId) {
      this.selectedId = this.selectedId ? '' : selectedId
      nextTick(() => {
        this.$refs.edit.focus()
      })  
    },
    editTodoText(event, id) {
      if (event.key === 'Escape' || event.target.value === '' ) {
        this.selectedId = ''
        return
      }
      
      this.$emit('onEdit', event.target.value, id)
      event.target.value = ''
      this.selectedId = ''
    }
  }
}
</script>

<style lang="css" scoped>
.completed{
  text-decoration-line: line-through;
}
  
</style>