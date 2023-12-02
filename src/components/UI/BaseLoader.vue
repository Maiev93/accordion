<script setup></script>

<template>
  <div class="loader" @wheel.prevent @touchmove.prevent @scroll.prevent>
    <div class="loader_dual-ring"></div>
  </div>
</template>

<style lang="scss" scoped>
.loader {
  background: rgba(0, 0, 0, 0.2);
  width: 100vw;
  height: 100vh;
  position: absolute;
  z-index: 1;
  display: flex;
  justify-content: center;
  align-items: center;

  &_dual-ring {
    display: inline-block;
    width: 80px;
    height: 80px;
  }

  &_dual-ring:after {
    content: " ";
    display: block;
    width: 64px;
    height: 64px;
    margin: 8px;
    border-radius: 50%;
    border: 6px solid #fff;
    border-color: #fff transparent #fff transparent;
    animation: loader_dual-ring 1.2s linear infinite;
  }

  @keyframes loader_dual-ring {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
}
</style>
