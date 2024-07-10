<template>
  <section class="add-todo">
    <div
        class="add-todo__item">
      <div class="text-input text-input--focus">
        <input
            :value="modelValue"
            @input="updateValue($event.target.value)"
            class="input"/>
      </div>
      <button class="button button--filled"
              @click="$emit('addNewToDo')">Submit
      </button>
    </div>
  </section>
</template>

<script setup lang="ts">
import {defineEmits} from "vue";

const emit = defineEmits([
  'addNewToDo',
  'update:modelValue'
])

const updateValue = (value: string): void => {
  emit('update:modelValue', value);
}
defineProps({
  modelValue: {type: [Object, String], required: true}
})

;

</script>

<style scoped>
.add-todo {
  display: flex;
  gap: 1.2rem;
}

.add-todo__item {
  display: flex;
  flex-direction: column;
  align-content: center;
  align-items: center;
  gap: 1.6rem;
  width: 100%;
  padding: 1.4rem 1.6rem 1.4rem;
  border: 0.1rem solid var(--color-border);
  border-radius: 1.6rem;
  box-shadow: 0 1px 20px rgb(240 240 240 / 80%);
}

.add-todo__item {
  justify-self: end;
  font: var(--font-lg);
}

.text-input--focus {
  border: 1px solid var(--primary-color);
}

.text-input .input {
  display: flex;
  width: auto;
  padding: 0;
  border: none;
}

.text-input .input:focus {
  outline: none;
}

.button--filled {
  padding: 0.2rem .2rem;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 140px;
  color: #ffffff;
  background-color: var(--primary-color);
  border-radius: .6rem;
}

</style>