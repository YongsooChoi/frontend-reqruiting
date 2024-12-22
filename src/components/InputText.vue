<script setup lang="ts">
import { defineEmits } from "vue";

type InputTextProps = {
  label: string;
  error?: {
    message: string;
  };
};

defineProps<InputTextProps>();

const emits = defineEmits(["input"]);
const model = defineModel<string>();

const focusPreviousInput = (currentTarget: HTMLElement) => {
  if (currentTarget?.previousElementSibling) {
    const inputElement =
      currentTarget.previousElementSibling as HTMLInputElement;
    inputElement.focus();
  }
};

const clearInput = (event: MouseEvent) => {
  model.value = "";
  emits("input", "");

  const currentTarget = event.currentTarget as HTMLElement;
  focusPreviousInput(currentTarget);
};

const handleInput = (event: Event) => {
  const inputElement = event.target as HTMLInputElement;
  emits("input", inputElement.value);
};
</script>

<template>
  <label class="flex flex-col gap-4 relative">
    {{ label }}
    <input
      class="border-gray-400 border rounded-md h-10 pl-4 pr-8 hover:border-blue-400 invalid:border-red-500 focus:border-green-400 focus:outline-none"
      type="text"
      autocomplete="off"
      v-model="model"
      @input="handleInput"
    />
    <button
      v-if="model && model.length > 0"
      type="button"
      class="absolute right-4 top-3/4 transform -translate-y-1/2 cursor-pointer"
      @click="clearInput"
    >
      X
    </button>
  </label>
  <span class="error" v-if="error">{{ error.message }}</span>
</template>

<style scoped>
span.error {
  color: red;
}
</style>
