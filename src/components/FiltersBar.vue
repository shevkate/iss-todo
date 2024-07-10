<template>
  <aside class="filters">
    <section class="button-group">
      <button
          v-for="filter in filters"
          :key="filter"
          class="button"
          :class="{ 'button--primary': activeFilter === filter }"
          @click="$emit('setFilter', filter)"
      >{{ filter }}
      </button>
    </section>
  </aside>
</template>

<script setup lang="ts">
import {Filter} from '@/types/Filter';
import {PropType, defineProps, defineEmits} from 'vue';

const filters: Filter[] = ['All', 'Active', 'Done'];

defineProps({
  activeFilter: {type: String as PropType<Filter>, required: true}
});
defineEmits([
  'setFilter'
]);

</script>

<style scoped>
.filters {
  display: flex;
  flex-direction: column;
  gap: 1.6rem;
}

.button-group {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  box-sizing: border-box;
  border: 0.1rem solid var(--color-border);
  border-radius: 1.6rem;
}

.button {
  font-size: 1.6rem;
  padding: 0.6rem 1rem;
  border-radius: 1.6rem;
}

.button--primary {
  color: var(--primary-color);
  border: 2px solid var(--primary-color);
}
</style>