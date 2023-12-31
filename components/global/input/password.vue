<script setup>
import { computed, toRef, useSlots, ref } from "vue";
import { useField } from "vee-validate";

const props = defineProps({
  value: {
    type: String,
    default: "",
  },
  name: {
    type: String,
    required: true,
  },
  label: {
    type: String,
    required: true,
  },
  placeholder: {
    type: String,
    default: "",
  },
  icon: {
    type: String,
    default: "",
  },
  disabled: {
    type: Boolean,
    default: false,
  },
  readonly: {
    type: Boolean,
    default: false,
  },
  primary: {
    type: Boolean,
    default: false,
  },
});
const name = toRef(props, "name");
const showPassword = ref(true);
const {
  value: inputValue,
  errorMessage,
  handleBlur,
  handleChange,
  meta,
} = useField(name);
</script>

<template>
  <div class="mb-6 last:mb-0" :class="{ success: meta.valid }">
    <label
      v-if="label"
      :for="name"
      class="form-label block mb-2"
      :class="{
        'text-red-500 font-bold': !!errorMessage,
      }"
    >
      <span v-if="primary">
        <span class="text-red-500 font-bold">*</span>
      </span>
      {{ label }}</label
    >
    <div class="relative">
      <input
        :id="name"
        :name="name"
        :type="showPassword ? 'password' : 'text'"
        class="form-control w-full block"
        :class="{
          'border-red-500 dark:border-red-500': !!errorMessage,
          'pl-10': !!icon,
        }"
        :disabled="disabled"
        :readonly="readonly"
        :value="inputValue"
        :placeholder="placeholder"
        @input="handleChange"
        @blur="handleBlur"
      />

      <!-- <BaseIcon
        v-if="icon"
        :path="icon"
        w="w-10"
        h="h-12"
        class="absolute top-0 left-0 z-10 pointer-events-none text-gray-500 dark:text-slate-400"
      /> -->
      <div
        class="absolute inset-y-0 right-0 flex items-center px-3 text-gray-600"
      >
        <component
          :is="showPassword ? 'EyeIcon' : 'EyeOffIcon'"
          @click="showPassword = !showPassword"
        />
      </div>

      <div
        v-if="icon"
        class="absolute inset-y-0 left-0 flex items-center px-3 text-gray-600"
      >
        <component :is="icon" />
      </div>
    </div>
    <div
      v-if="!!errorMessage"
      class="text-xs text-red-500 dark:text-red-500 mt-1"
    >
      {{ errorMessage }}
    </div>
  </div>
</template>

<style lang="scss" scoped></style>
