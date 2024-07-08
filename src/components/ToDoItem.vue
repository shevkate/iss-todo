<script setup lang="ts">
import { Todo } from '@/types/Todo';
import {PropType, defineEmits, defineProps} from 'vue';

defineProps({
  todo: { type: Object as PropType<Todo>, required: true}

})

defineEmits([
    'clickTodo',
    'removeTodo',
    'editTodo'
])

</script>

<template>
  <li class="todo-item"
      :class="{'todo-item--done': todo.done}"
      @click="$emit('clickTodo', todo.id)">
    <div class="todo-item__status">
      <i class="bi bi-check2"></i>
    </div>
    <span class="todo-item__text">{{ todo.text }}</span>
    <button class="todo-item__button" @click.stop="$emit('removeTodo', todo.id)">
      <i class="bi bi-trash3"></i>
    </button>
    <button class="todo-item__button" @click.stop="$emit('editTodo', todo)">
      <i class="bi bi-pencil"></i>
    </button>
  </li>
</template>

<style scoped>
.todo-item {
  display: grid;
  grid-template-columns: 2.8rem 1fr 1.6rem 1.6rem;
  align-items: center;
  gap: 1.6rem;
  padding: 1.6rem 2rem;
  border: 0.1rem solid var(--color-border);
  border-radius: 1.6rem;
  transition: box-shadow 0.2s;
  cursor: pointer;
  font-size: 1.6rem;
}

.todo-item:hover {
  box-shadow: 0 1px 20px rgb(240 240 240 / 80%);
}

.todo-item--done .todo-item__text {
  text-decoration: line-through;
}

.todo-item__status {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 2.4rem;
  height: 2.4rem;
  font-size: 2.4rem;
  opacity: 0;
  transition: opacity 0.2s;
}

.todo-item__status .bi-check2 {
  position: relative;
  top: 0.1rem;
  color: var(--primary-color)
}

.todo-item--done .todo-item__status {
  opacity: 1;
}

.todo-item:hover .todo-item__status {
  opacity: 1;
}

.todo-item--done .todo-item__status .bi-check2 {
  display: block;
}

.todo-item__button
{
  opacity: 0;
  color: var(--color-border);
  transition: opacity 0.2s;
}

.todo-item:hover .todo-item__button {
  opacity: 1;
}

.todo-item__button:hover  {
  color: var(--primary-color);
}
</style>