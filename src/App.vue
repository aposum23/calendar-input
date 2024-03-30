<script setup lang="ts">
import InputData from "@/components/InputData.vue";
import {ref} from "vue";

const selectedLang = ref<string>('rus');
</script>

<template>
  <div class="form">
    <InputData :selected-lang="selectedLang"/>
    <select v-model="selectedLang">
      <option value="rus" selected>Русский язык</option>
      <option value="eng">Английский язык</option>
    </select>
  </div>
</template>

<style scoped>
.form {
  display: grid;
  grid-template-columns: 20rem 11rem;
}

select {
  height: 1.4rem;
  margin-top: 1rem;
  font-size: 1.1rem;
}
</style>
