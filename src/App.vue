<template>
  <div class="flex flex-col justify-center items-center h-screen">
    <div>
      <h1 class="text-5xl">Crawler</h1>
    </div>
    <app-card class="max-w-sm">
      <base-input label="URL" v-model="url"></base-input>
      <div class="flex">
        <base-button :loading="loading" @click="getPage">Button</base-button>
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
import BaseButton from "./components/BaseButton.vue";
import AppCard from "./components/AppCard.vue";

export default {
  components: { BaseInput, AppCard, BaseButton },
  data() {
    return {
      url: "",
      message: { text: "", status: "", success: true, active: false },
      refreshIframe: 0,
      loading: false,
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
      this.loading = true;
      try {
        response = await fetch(this.url, {
          mode: "no-cors",
        });
        this.message.text = "Success";
        this.message.success = true;
      } catch (error) {
        this.message.text = "Error";
        this.message.success = false;
      }
      this.message.status = response.status;
      this.message.active = true;
      this.refreshIframe++;
      this.loading = false;
    },
  },
};
</script>

<style>
body {
  background-color: #f0fff8;
}

.button {
  background-color: #96e6b3;
}
</style>
