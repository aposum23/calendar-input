<script setup lang="ts">
defineProps<{
  arrowDirection: 'left' | 'right';
}>();

const emit = defineEmits<{
  "change-month": [number];
}>()
</script>

<template>
  <div
      :class="arrowDirection === 'left' ? 'left-arrow' : 'right-arrow' "
      @click="emit('change-month', arrowDirection === 'left' ? -1 : 1)"
  ></div>
</template>

<style scoped>
.left-arrow {
  width: .5rem;
  height: .5rem;
  transform: rotate(180deg);
}

.right-arrow {
  width: .5rem;
  height: .5rem;
}

.left-arrow, .right-arrow {
  padding: .2rem;
  background-image: url("@/assets/images/arrow.svg");
  background-position: center;
  background-repeat: no-repeat;
  background-clip: padding-box;

}

.left-arrow:hover, .right-arrow:hover {
  cursor: pointer;
}

.left-arrow:active, .right-arrow:active {
  border: 1px solid #65afdc;
}
</style>