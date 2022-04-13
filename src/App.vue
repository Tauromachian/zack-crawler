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
          method: "POST",
          mode: "no-cors",
          headers: {
            "Content-Type": "application/x-www-form-urlencoded",
            // "Content-Type": "multipart/form-data",
          },
          body: JSON.stringify({
            __EVENTTARGET: "lnkBrowseEmpty",
            __EVENTARGUMENT: "",
            __VIEWSTATE:
              "%2FwEPDwUKMTYwMzk5MTA4OA9kFgICAQ9kFhgCAQ8PFgIeBFRleHQFBkxvZyBJbmRkAgIPDxYCHgdFbmFibGVkaGRkAgUPDxYCHwFoZGQCBg8PFgIfAAUNTm90IExvZ2dlZCBJbmRkAgcPDxYCHwAFICAgSW5hY3RpdmUgaW4gUGFpZCBEZXRhaWwgU3lzdGVtZGQCCA8PFgIfAGVkZAIKDw8WAh8ABZkDUE8vRGV0J3MgbWFya2VkIGFzICdhY3RpdmUnIGluIHRoZSBQYWlkIERldGFpbCBTeXN0ZW0gbWF5IHNjaGVkdWxlIGluIGFkdmFuY2UgdXNpbmcgdGhpcyBwcm9ncmFtIGlmIHRoZSB5ZWFyIG9mIHRoZWlyIGFwcG9pbnRtZW50IGRhdGUgaXM6IDE5ODEgLSAyMDIwLjxiciAvPlRoZSAjIG9mIGhvdXJzIHRoYXQgY2FuIGJlIHNjaGVkdWxlZCBmb3IgZWFjaCByYW5rIHVzaW5nIHRoaXMgYXBwIGFyZSBjdXJyZW50bHk6IFBPL0RldDogODAgaG91cnMuIFNndDogMzIgaG91cnMuIEx0OiAzMiBob3Vycy48YnIgLz5QbGVhc2UgdmlldyB0aGUgZGlyZWN0aW9ucyA8YSBocmVmPSJmcm1EaXJlY3Rpb25zLmFzcHgiPiBoZXJlIDwvYT4gYW5kIG9uIHRoZSBQb3J0YWwgZm9yIGNvbXBsZXRlIGV4cGxhbmF0aW9uIG9mIHJ1bGVzLmRkAgsPZBYGAgEPZBYCAgEPDxYCHgdWaXNpYmxlaGRkAgUPDxYCHwJoZGQCBw8PFgIfAAUTICAgIFBhZ2U6IE1haW4gTWVudWRkAgwPDxYCHwFoZGQCDg8PFgIfAmhkZAIQDw8WAh8CaGRkAhQPDxYIHglCYWNrQ29sb3IKHB4LQm9yZGVyU3R5bGULKiVTeXN0ZW0uV2ViLlVJLldlYkNvbnRyb2xzLkJvcmRlclN0eWxlBB4LQm9yZGVyV2lkdGgbAAAAAAAA8D8BAAAAHgRfIVNCAmhkFgQCAQ8PFgIfAAUGTk9USUNFZGQCAw8PFgIfAAXZAU1lbWJlcnMgYXBwcm92ZWQgZm9yIHRlbGV3b3JrIG9yIHdpdGggYW4gYXBwcm92ZWQgY292aWQtMTkNCnJlYXNvbmFibGUgYWNjb21tb2RhdGlvbiBhcmUgbm90IHBlcm1pdHRlZCB0byBwYXJ0aWNpcGF0ZSBpbiBvZmYtZHV0eQ0KZW1wbG95bWVudCBvciB0aGUgcGFpZCBkZXRhaWwgcHJvZ3JhbSB1bnRpbCBzdWNoIHJlcXVlc3QvYXBwcm92YWwgaGFzIGJlZW4NCndpdGhkcmF3bi5kZGQmKMEaXUQbc4EOF7bXmKcvx8uMkz%2FxPoVWwtuSYcXzNg%3D%3D",
            __VIEWSTATEGENERATOR: "3C18D2D0",
            __EVENTVALIDATION:
              "%2FwEdAAW%2B8o%2FFHrmTNyaCChcQXORw4IsXNaS72srkKEKfKITh%2F010wlQM8Fa2oZAXS6YymIav6Xmx%2FranT48GGlnL%2BIEwqfbU8NPC0IdoChCieaJ3jO8Q9xlt4AZtjHjgVddKTnaMZ0J2HnQ0dzRe3L5LJdni",
          }),
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
