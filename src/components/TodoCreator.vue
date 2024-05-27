<script setup>
import { ref } from 'vue'
import TodoButton from './TodoButton.vue';

const emit = defineEmits(['create-todo'])

const todoState = ref({
  todo: '',
  invalid: null,
  errMsg: ''
})

const createTodo = () => {
  todoState.value.invalid = null
  if (todoState.value.todo !== '') {
    emit('create-todo', todoState.value.todo)
    todoState.value.todo = ''
    return
  }
  todoState.value.invalid = true
  todoState.value.errMsg = 'Todo value cannot be empty'
}
</script>

<template>
  <div class="input-wrap" :class="{ 'input-err': todoState.invalid }">
    <input type="text" v-model="todoState.todo" />
    <TodoButton @click="createTodo"/>
  </div>
  <p v-show="todoState.invalid" class="err-msg">{{ todoState.errMsg }}</p>
</template>

<style lang="scss" scoped>
.input-wrap {
  display: flex;
  transition: 250ms ease;
  border: 2px solid #41b080;

  &.input-err {
    border-color: red;
  }

  &:focus-within {
    box-shadow:
      0 -4px 6px -1px rgb(0 0 0 / 0.1),
      0 -2px 4px -2px rgb(0 0 0 / 0.1);
  }

  input {
    width: 100%;
    padding: 8px 6px;
    border: none;

    &:focus {
      outline: none;
    }
  }
}

.err-msg {
  margin-top: 6px;
  font-size: 12px;
  text-align: center;
  color: red;
}
</style>
