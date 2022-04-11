<template>
  <div class="flex flex-col justify-left">
    <label
      for="exampleFormControlInput1"
      class="form-label inline-block mb-2 text-gray-700 text-left"
      >{{ label }}</label
    >
    <input
      v-bind="$attrs"
      v-model="text"
      type="text"
      class="form-control block w-full px-4 py-2 text-xl font-normal text-gray-700 bg-white bg-clip-padding border border-solid border-gray-300 rounded transition ease-in-out m-0 focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none max-w-max"
      id="exampleFormControlInput2"
      placeholder="Form control lg"
    />
  </div>
</template>

<script>
export default {
  name: "BaseInput",
  props: {
    modelValue: {
      type: String,
      default: "",
    },
    label: {
      type: String,
      default: "",
    },
  },
  text: {
    get() {
      return this.modelValue;
    },
    set(val) {
      this.$emit("input", val);
    },
  },
};
</script>

<style></style>
