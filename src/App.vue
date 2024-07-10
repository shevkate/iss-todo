<template>
  <Header/>
  <FiltersBar
      :active-filter="activeFilter"
      @set-filter="setFilter"
  />
  <main class="main">
    <ToDoList
        v-model="selectedTodo.text"
        :todos="filteredTodos"
        @remove-todo="removeTodo"
        @edit-todo="editTodo"
        @click-todo="clickTodo"
    />
  </main>
  <NewToDo @add-new-to-do="addNewToDo" v-model="editedTodo.text"/>
  <Footer :status="status"/>
</template>

<script setup lang="ts">
import Header from "@/components/layout/Header.vue";
import Footer, {Status} from "@/components/layout/Footer.vue";
import ToDoList from "@/components/ToDoList.vue";
import FiltersBar from "@/components/FiltersBar.vue";
import {Todo} from './types/Todo';
import {Filter} from './types/Filter';
import {computed, ref, onMounted, ComputedRef, Ref, watch, UnwrapRef} from "vue";
import NewToDo from "@/components/NewToDo.vue";

let todos: Ref<Todo[]> = ref(
    [{id: 0, text: 'Learn the basics of Vue', completed: true},
      {id: 1, text: 'Learn the basics of Typescript', completed: false},
      {id: 2, text: 'Subscribe to the channel', completed: false}]
);

const baseTodo: Todo = {
  id: Date.now(),
  text: '',
  done: false
};

let activeFilter: Ref<Filter> = ref('All');

const activeTodos: ComputedRef<Todo[]> = computed(() => {
  return todos.value.filter((todo: Todo) => !todo.done)
});
const doneTodos: ComputedRef<Todo[]> = computed(() => {
  return todos.value.filter((todo: Todo) => todo.done)
});

const filteredTodos: ComputedRef<Todo[]> = computed(() => {
  switch (activeFilter.value) {
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

let editedTodo: Ref<UnwrapRef<{}>> = ref({});
let selectedTodo: Ref<UnwrapRef<{}>> = ref({});

const editTodo = (todo: Todo): void => {
  selectedTodo.value = todo
  editedTodo.value = JSON.parse(JSON.stringify(todo))
}

const addNewToDo = () => {
// Editing existed
  if (selectedTodo.value.text) {
    selectedTodo.value.text = editedTodo.value.text;
    console.log(selectedTodo.value)
    editedTodo.value = {};
    selectedTodo.value = {};
  } else {
// Created new one
    (<any>Object).assign(baseTodo, editedTodo.value);
    todos.value.push(baseTodo)
    editedTodo.value = {}
  }
}

const clickTodo = (id: number): void => {
  const selectedTodo: Todo = todos.value.filter((todo: Todo) => todo.id === id)[0];
  if (selectedTodo) {
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

onMounted(() => {
      const savedData = JSON.parse(localStorage.getItem('todos'));
      if (savedData) {
        todos.value = savedData
      }
    }
)
watch(() => todos.value,
    (newVal, oldVal) => {
      localStorage.setItem('todos', JSON.stringify(newVal))

    }, {deep: true}
);


</script>

<style scoped>
.main {
  display: grid;
  gap: 1.6rem;
}
</style>
