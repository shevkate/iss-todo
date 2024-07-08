<template>
  <section class="add-todo">
    <div
        v-if="isVisible"
        class="add-todo__item"
        @submit.prevent="$emit('addNewToDo',{
          id: Date.now(),
          text: todoText,
          done: false
        }, todoText = '')">
      <button
          class="close-button"
          type="button"
          @click="closeNewToDo"
      >
        <i class="bi bi-x"></i>
      </button>
      <div class="text-input text-input--focus">
        <input v-model="todoText" class="input" />
      </div>
      <button class="button button--filled">Add task</button>
    </div>
    <button
        v-else
        class="add-todo__show-item-button"
        @click="showNewToDo"
    >
      <i class="bi bi-plus-lg"></i>
    </button>
  </section>
</template>

<script setup lang="ts">
import { Todo } from '@/types/Todo';
import {defineEmits, ref} from "vue";
import type { Ref, UnwrapRef } from 'vue'

let isVisible: Ref<UnwrapRef<boolean>> = ref(false);

let todoText: Ref<UnwrapRef<string>> = ref('');

const showNewToDo = () => {
  isVisible.value = true
};

const closeNewToDo = () => {
  isVisible.value = false
}

  defineEmits([
    'addNewToDo',
  ])

</script>

<style scoped>
.add-todo {
  display: grid;
  gap: 1.6rem;
}

.add-todo__show-item-button {
  grid-template-columns: 1fr;
  justify-items: center;
  padding: 1.2rem 1.4rem;
  color: var(--primary-color);
  font-size: 1.9rem;
  border: 0.1rem solid var(--color-border);
  border-radius: 1.6rem;
  transition: box-shadow 0.2s;
}

.add-todo__show-item-button:hover {
  box-shadow: 0 1px 20px rgb(240 240 240 / 80%);
}

.add-todo__item {
  display: grid;
  gap: 1.6rem;
  padding: 1.4rem 1.6rem 2rem;
  border: 0.1rem solid var(--color-border);
  border-radius: 1.6rem;
  box-shadow: 0 1px 20px rgb(240 240 240 / 80%);
}

.add-todo__item .close-button {
  justify-self: end;
  font-size: 2rem;
}

.add-todo__form .close-button:hover {
  color: var(--primary-color);
}

.text-input--focus {
  border: 1px solid var(--primary-color);
}

.text-input .input {
  flex-grow: 1;
  padding: 0;
  border: none;
}

.text-input .input:focus {
  outline: none;
}


</style>