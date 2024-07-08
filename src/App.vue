<template>
<Header/>
  <FiltersBar
      :active-filter="activeFilter"
      @set-filter="setFilter"
  />
  <main class="main">
<ToDoList
    :todos="filteredTodos"
    @click-todo="clickTodo"
    @remove-todo="removeTodo"
/>
  </main>
  <Footer :status="status"/>
</template>

<script setup lang="ts">
import Header from "@/components/layout/Header.vue";
import Footer, {Status} from "@/components/layout/Footer.vue";
import ToDoList from "@/components/ToDoList.vue";
import FiltersBar from "@/components/FiltersBar.vue";
import { Todo } from './types/Todo';
import { Filter } from './types/Filter';
import {computed, ref, Ref} from "vue";
import type {  ComputedRef } from 'vue'

let items: Ref<Todo[]> | ComputedRef<Todo[]> | Todo[] = [
  { id: 0, text: 'Pet a cat', done: true },
  { id: 1, text: 'Pet a dog', done: true },
  { id: 2, text: 'Pet a capybara', done: false },
];
let todos: Ref<Todo[]> = ref(items);

let activeFilter: Ref<Filter> = ref('All');


const activeTodos: ComputedRef<Todo[]> = computed(() => {
  return todos.value.filter((todo: Todo) => !todo.done)
});

const doneTodos: ComputedRef<Todo[]> = computed(() => {
  return todos.value.filter((todo: Todo) => todo.done)
});

const filteredTodos: ComputedRef<Todo[]> | Ref<Todo[]>  = computed(() => {
  switch(activeFilter.value) {
    case 'Active':
      return activeTodos.value
    case 'Done':
      return doneTodos.value
    case 'All':
    default:
      return todos.value
  }
})

const status: ComputedRef<Status> = computed(() => {
  return {
    active: activeTodos.value.length,
    done: doneTodos.value.length,
  }
})

const addTodo = (todo: Todo): void => {
  todos.value.push(todo)
}

const clickTodo = (id: number): void => {
  const selectedTodo: Todo = todos.value.filter((todo: Todo) => todo.id === id)[0];
  if(selectedTodo) {
    selectedTodo.done = !selectedTodo.done
  }
  return;
}

const removeTodo = (id: number) => {
  todos.value = todos.value.filter((todo: Todo) => todo.id !== id)
}

const setFilter = (filter: Filter): void => {
  activeFilter.value = filter
}
</script>

<style scoped>
.main {
    display: grid;
    gap: 1.6rem;
}
</style>
