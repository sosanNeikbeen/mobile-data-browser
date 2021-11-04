<template>
  <button :class="className" @click="$emit('click', $event)">
    <slot></slot>
  </button>
</template>

<script>
export default {
  props: ["className"],
};
</script>

<style scoped></style>
