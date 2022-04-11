<template>
  <div class="flex flex-col justify-center items-center h-screen">
    <div>
      <h1>Crawler</h1>
    </div>
    <app-card class="max-w-sm">
      <base-input label="URL" v-model="url"></base-input>
      <div class="flex">
        <button
          @keydown.enter="getPage"
          @click="getPage"
          type="button"
          class="button inline-block px-6 py-2 text-2xs rounded shadow-md hover:shadow-lg active:shadow-lg mt-5"
        >
          Button
        </button>
      </div>
    </app-card>

    <app-card v-if="message.active" class="container my-5 w-full h-full mx-5">
      <div>Status: {{ message.text }} Response: {{ message.status }}</div>
      <iframe
        sandbox
        :key="refreshIframe"
        v-if="message.success"
        class="w-full h-full"
        :src="url"
        name="targetframe"
        allowTransparency="true"
        scrolling="no"
        frameborder="0"
      >
      </iframe>
    </app-card>
  </div>
</template>

<script>
import BaseInput from "./components/BaseInput.vue";
import AppCard from "./components/AppCard.vue";

export default {
  components: { BaseInput, AppCard },
  data() {
    return {
      url: "",
      message: { text: "", status: "", success: true, active: false },
      refreshIframe: 0,
    };
  },
  mounted() {
    const body = document.body;
    body.addEventListener("keyup", (event) => {
      if (event.key === "Enter") {
        this.getPage();
      }
    });
  },
  methods: {
    async getPage() {
      let response;
      try {
        response = await fetch(this.url);
        this.message.text = "Success";
        this.message.success = true;
      } catch (error) {
        this.message.text = "Error";
        this.message.success = false;
      }
      this.message.status = response.status;
      this.message.active = true;
      this.refreshIframe++;
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
