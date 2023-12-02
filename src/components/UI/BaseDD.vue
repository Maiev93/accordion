<script setup>
import { computed, ref, onMounted } from "vue";

const props = defineProps({
  titleDD: { type: String, default: "Title" },
  index: { type: Number, default: null },
  opened: { type: Array, default: () => [] },
  isAllOpened: { type: Boolean, default: false },
});

const isContentShown = ref(false);

onMounted(() => {
  setDefaultContentState();
});

const imageURL = computed(() => {
  return isContentShown.value
    ? new URL("../../assets/icons/arrow-down.svg", import.meta.url).href
    : new URL("../../assets/icons/arrow-right.svg", import.meta.url).href;
});

const setDefaultContentState = () => {
  if (props.opened.includes(props.index)) {
    isContentShown.value = true;
  } else if (props.isAllOpened) {
    isContentShown.value = true;
  }
};
const toggleContent = () => {
  isContentShown.value = !isContentShown.value;
};
</script>

<template>
  <div class="base-dropdown">
    <div class="base-dropdown__header">
      <p class="base-dropdown__title">{{ titleDD }}</p>
      <img
        :src="imageURL"
        alt="open/close"
        class="base-dropdown__img"
        @click="toggleContent"
      />
    </div>
    <transition name="unroll">
      <div class="base-dropdown__content" v-if="isContentShown">
        <slot />
      </div>
    </transition>
  </div>
</template>

<style lang="scss" scoped>
.base-dropdown {
  border-radius: 0.5rem;
  //   border: 1px solid grey;
  padding: 0.5rem;
  background: rgba(201, 201, 201, 0.5);

  &__header {
    display: flex;
    width: 100%;
    align-items: center;
  }

  &__title {
    flex-grow: 1;
  }

  &__img {
    width: 1rem;
    height: 1rem;
  }
}

.unroll-enter-active,
.unroll-leave-active {
  transition: opacity 1s ease;
}
.unroll-enter-from,
.unroll-leave-to {
  opacity: 0;
}
</style>
