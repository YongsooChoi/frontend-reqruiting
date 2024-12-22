<script setup lang="ts">
import { computed, ref } from "vue";
import InputText from "./InputText.vue";

const userId = ref("");
const name = ref("");
const organization = ref("");
const errors = ref({
  userId: "",
  name: "",
  organization: "",
});

const validateUserId = (value: string) => {
  const regex = /^[a-zA-Z0-9]+$/;
  if (!value) {
    errors.value.userId = "반드시 작성되어야 합니다.";
  } else if (!regex.test(value)) {
    errors.value.userId = "알파벳과 숫자만 허용합니다.";
  } else {
    errors.value.userId = "";
  }
};

const validateName = (value: string) => {
  const regex = /[\\/:*?"<>|]/;
  if (!value) {
    errors.value.name = "반드시 작성되어야 합니다.";
  } else if (regex.test(value)) {
    errors.value.name = '다음 특수문자가 허용되지 않습니다. \\ / : * ? " < > |';
  } else {
    errors.value.name = "";
  }
};

const validateOrganization = (value: string) => {
  const regex = /[\\/:*?"<>|]/;
  if (value && regex.test(value)) {
    errors.value.organization =
      '다음 특수문자가 허용되지 않습니다. \\ / : * ? " < > |';
  } else {
    errors.value.organization = "";
  }
};

const isFormValid = computed(() => {
  return (
    !errors.value.userId &&
    !errors.value.name &&
    !errors.value.organization &&
    userId.value &&
    name.value
  );
});

const handleSubmit = () => {
  if (isFormValid.value) {
    alert(`User ID: ${userId.value}
Name: ${name.value}
Organization: ${organization.value}`);
  }
};
</script>

<template>
  <form class="flex flex-col gap-4 py-4" @submit.prevent="handleSubmit">
    <InputText
      label="User ID"
      v-model="userId"
      @input="validateUserId"
      :error="errors.userId ? { message: errors.userId } : undefined"
    />
    <InputText
      label="Name"
      v-model="name"
      @input="validateName"
      :error="errors.name ? { message: errors.name } : undefined"
    />
    <InputText
      label="Organization"
      v-model="organization"
      @input="validateOrganization"
      :error="
        errors.organization ? { message: errors.organization } : undefined
      "
    />
    <button
      class="px-4 py-2 rounded-md"
      :class="{
        'bg-blue-500 text-white': isFormValid,
        'bg-gray-400 text-gray-700 cursor-not-allowed': !isFormValid,
      }"
      type="submit"
      :disabled="!isFormValid"
    >
      Submit
    </button>
  </form>
</template>
