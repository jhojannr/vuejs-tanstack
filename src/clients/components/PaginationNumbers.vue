<script setup lang="ts">
import { toRef, ref, watch } from "vue";

interface Props {
  totalPages: number;
  currentPage: number;
}

interface Emits {
  (e: "pageChanged", page: number): void;
}

const emits = defineEmits<Emits>();
const props = defineProps<Props>();

const totalPages = toRef(props, "totalPages");
const currentPage = toRef(props, "currentPage");

const totalPageNumbers = ref<number[]>([]);

watch(
  totalPages,
  () => {
    totalPageNumbers.value = [...new Array(totalPages.value)].map(
      (value, index) => index + 1
    );
  },
  { immediate: true }
);
</script>

<template>
  <div>
    <button
      :disabled="currentPage === 1"
      @click="emits('pageChanged', currentPage - 1)"
    >
      Anterior
    </button>
    <button
      v-for="number of totalPageNumbers"
      :key="number"
      :class="{ active: currentPage === number }"
      @click="emits('pageChanged', number)"
    >
      {{ number }}
    </button>
    <button
      :disabled="currentPage === totalPages"
      @click="emits('pageChanged', currentPage + 1)"
    >
      Siguiente
    </button>
  </div>
</template>

<style scoped>
div {
  margin: 0 auto;
  text-align: center;
}

.active {
  background-color: hsla(160, 100%, 37%, 0.2);
}
</style>
