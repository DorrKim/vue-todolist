<template>
  <form @submit.prevent="submit">
    <input
      v-model.trim="newTodoText" 
      placeholder="할일을 입력해 주세요" />
    <button>추가하기</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      newTodoText: ''
    }
  },
  methods: {
    submit() {
      this.$emit('onSubmit', this.newTodoText)
      this.newTodoText = ''
    }
  }
}
</script>
