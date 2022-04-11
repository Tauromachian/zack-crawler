<template>
  <button
    @keydown.enter="getPage"
    v-bind="$attrs"
    @click="getPage"
    type="button"
    class="button inline-block px-6 py-2 text-2xs rounded shadow-md hover:shadow-lg active:shadow-lg mt-5"
  >
    <slot></slot>
  </button>
</template>

<script>
export default {};
</script>

<style></style>
