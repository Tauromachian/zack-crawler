<template>
  <div class="flex flex-col justify-center items-center h-screen">
    <div>
      <h1>Crawler</h1>
    </div>
    <div class="block p-6 rounded-lg shadow-lg bg-white max-w-sm">
      <base-input label="URL" v-model="url"></base-input>
      <div class="flex">
        <button
          type="button"
          class="button inline-block px-6 py-2 text-2xs rounded shadow-md hover:shadow-lg active:shadow-lg mt-5"
        >
          Button
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import BaseInput from "./components/BaseInput.vue";

export default {
  components: { BaseInput },
  data() {
    return {
      url: "",
    };
  },
};
</script>

<style lang="scss">
body {
  background-color: #f0fff8;
}

.button {
  background-color: #42b983;
}
</style>
