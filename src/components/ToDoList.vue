<script setup lang="ts">
import {defineEmits, defineProps, PropType} from "vue";
import {Todo} from "@/types/Todo.js";
import ToDoItem from "@/components/ToDoItem.vue";

defineProps({
  todos: {type: Array as PropType<Todo[]>, required: true},
    })

defineEmits([
  'clickTodo',
  'removeTodo'
])

</script>

<template>
<ul class="todo-list">
  <ToDoItem
      v-for="todo in todos"
      :key="todo.id"
      :todo="todo"
      @click-todo="$emit('clickTodo', todo.id)"
      @remove-todo="$emit('removeTodo', todo.id)"
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