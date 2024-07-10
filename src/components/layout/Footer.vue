<script setup lang="ts">
import {PropType, defineProps} from "vue";

export interface Status {
  active: number,
  done: number
}

defineProps({
  status: {type: Object as PropType<Status>, required: true}
})

const pluralizeString = (count: number, noun: string, suffix = 's') =>
    `${noun}${count !== 1 ? suffix : ''}`;

</script>

<template>
  <section class="footer">
  <span>
    You have finished {{ status.done }} {{ pluralizeString(status.done, 'task') }}!
  </span>
  </section>
</template>

<style scoped>
.footer {
  font-size: 1.8rem;
  text-align: center;
}
</style>