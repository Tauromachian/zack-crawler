<template>
  <div class="flex flex-col justify-center items-center h-screen">
    <div>
      <h1>Crawler</h1>
    </div>
    <div class="block p-6 rounded-lg shadow-lg bg-white max-w-sm">
      <base-input label="URL" v-model="url"></base-input>
      <div class="flex">
        <button
          @click="getPage"
          type="button"
          class="button inline-block px-6 py-2 text-2xs rounded shadow-md hover:shadow-lg active:shadow-lg mt-5"
        >
          Button
        </button>
      </div>
    </div>
    <div>Status: {{ status }} Response: {{ response }}</div>
    <iframe
      v-if="message.active && message.success"
      :src="url"
      name="targetframe"
      allowTransparency="true"
      scrolling="no"
      frameborder="0"
    >
    </iframe>
  </div>
</template>

<script>
import BaseInput from "./components/BaseInput.vue";

export default {
  components: { BaseInput },
  data() {
    return {
      url: "",
      response: "",
    };
  },
  methods: {
    async getPage() {
      let response;
      try {
        response = await fetch(this.url);
        this.response = response;
      } catch (error) {
        this.response = response;
      }
    },
  },
};
</script>

<style lang="scss">
body {
  background-color: #f0fff8;
}

.button {
  background-color: #96e6b3;
}
</style>
