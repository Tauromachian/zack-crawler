<template>
  <button
    @keydown.enter="getPage"
    v-bind="$attrs"
    @click="getPage"
    type="button"
    class="button inline-block px-6 py-2 text-2xs rounded shadow-md hover:shadow-lg active:shadow-lg mt-5"
  >
    <div v-if="loading" class="flex justify-center items-center">
      <div
        class="spinner-border animate-spin inline-block w-6 h-6 border-4 border-top-primary rounded-full"
        role="status"
      />
      <span v-if="loadingText" class="ml-2">{{ loadingText }}</span>
      <span v-else class="ml-2">Loading...</span>
    </div>
    <slot v-else></slot>
  </button>
</template>

<script>
export default {
  name: "BaseButton",
  props: {
    loading: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style scoped>
.spinner-border {
  border-color: black !important;
}

.border-top-primary {
  border-top-color: #96e6b3 !important;
}
</style>
