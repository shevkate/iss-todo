<script setup lang="ts">
import {defineEmits, defineProps, PropType} from "vue";
import ToDoItem from "@/components/ToDoItem.vue";
import { Todo } from '@/types/Todo';

defineProps({
  todos: {type: Array as PropType<Todo[]>, required: true},
  modelValue: {type: [Object, String], required:true}
    })

const emits = defineEmits([
  'clickTodo',
  'removeTodo',
  'editTodo',
  'update:modelValue'
])

const updateValue = (value: string): void => {
  emits('update:modelValue', value);
}

</script>

<template>
<ul class="todo-list">
  <ToDoItem
      v-for="todo in todos"
      :key="todo.id"
      :todo="todo"
      :value="modelValue"
      @click="$emit('update:modelValue', modelValue);"
      @change="updateValue($event.target.value)"
      @click-todo="$emit('clickTodo', todo.id)"
      @remove-todo="$emit('removeTodo', todo.id)"
      @edit-todo="$emit('editTodo', todo)"
  />
</ul>
</template>

<style scoped>
.todo-list {
  display: grid;
  gap: 1.6rem;
  margin: 0;
  padding: 0;
  list-style: none;
}
</style>