<script setup>
defineProps({
  title: { type: String, default: "" },
});
</script>

<template>
  <h5 class="accordion__title">{{ title }}</h5>
  <div class="accordion__content">
    <slot />
  </div>
</template>

<style lang="scss" scoped>
.accordion {
  &__content {
    display: grid;
    grid-gap: 0.25rem;
  }
}
</style>
