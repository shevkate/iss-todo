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
    @click-todo="clickTodo"
    @remove-todo="removeTodo"
    @edit-todo="editTodo"
/>

    {{selectedTodo.text}}
    <br/>

    {'{edited'}}
    {{editedTodo.text}}
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
import {computed, ref, onMounted, ComputedRef, Ref, reactive} from "vue";
import NewToDo from "@/components/NewToDo.vue";

let items: Ref<Todo[]> | ComputedRef<Todo[]> | Todo[] = [
  { id: 0, text: 'Pet a cat', done: true },
  { id: 1, text: 'Pet a dog', done: true },
  { id: 2, text: 'Pet a capybara', done: false },
];
let todos: Ref<Todo[]> = ref(items);

let clonedTodos: Todo[] = JSON.parse(JSON.stringify(items));

let activeFilter: Ref<Filter> = ref('All');

const activeTodos: ComputedRef<Todo[]> = computed(() => {
  return clonedTodos.filter((todo: Todo) => !todo.done)
});

const doneTodos: ComputedRef<Todo[]> = computed(() => {
  return clonedTodos.filter((todo: Todo) => todo.done)
});

const filteredTodos: ComputedRef<Todo[]> | Ref<Todo[]>  = computed(() => {
  switch(activeFilter.value) {
    case 'Active':
      return activeTodos.value
    case 'Done':
      return doneTodos.value
    case 'All':
    default:
      return clonedTodos
  }
})

const status: ComputedRef<Status> = computed(() => {
  return {
    active: activeTodos.value.length,
    done: doneTodos.value.length,
  }
})

let editedTodo: Ref<{}> = ref({});
let selectedTodo: {} = {};

const editTodo = (todo: Todo): void => {
  selectedTodo = (<any>Object).assign({}, todo);
  editedTodo.value = todo
  // console.log(selectedTodo)
  // console.log(editedTodo)
}

const addNewToDo = (todo: Todo): void => {
  // console.log(selectedTodo)
  // console.log(editedTodo)
  if(todo.text) {
    console.log(todo.text)
    console.log(editedTodo)
    let updatedTodo = JSON.parse(JSON.stringify(todo))
    console.log(updatedTodo)
   selectedTodo = JSON.parse(JSON.stringify(updatedTodo))
    selectedTodo.text = ''
    //console.log(selectedTodo)
  } else {
    clonedTodos.push(todo)
    localStorage.setItem('todos', JSON.stringify(clonedTodos));
  }
}

const clickTodo = (id: number): void => {
  const selectedTodo: Todo = clonedTodos.filter((todo: Todo) => todo.id === id)[0];
  if(selectedTodo) {
    selectedTodo.done = !selectedTodo.done
  }
  return;
}

const removeTodo = (id: number) => {
  todos.value = todos.value.filter((todo: Todo) => todo.id !== id)
  localStorage.setItem('todos', JSON.stringify(clonedTodos));
}

//
// const updateTodo = (todo: Todo): void => {
//   if(!editedTodo.value) {
//     return;
//   } else if (!todo.text) {
//     removeTodo(todo.id)
//   }
//   editedTodo.value = null;
//   todo.text = editedTodoText.value.trim();
// }

const setFilter = (filter: Filter): void => {
  activeFilter.value = filter
}

onMounted(() => {
  JSON.parse(localStorage.getItem('todos'));
})
</script>

<style scoped>
.main {
    display: grid;
    gap: 1.6rem;
}
</style>
