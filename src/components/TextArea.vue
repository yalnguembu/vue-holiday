<template>
  <div>
    <label for="description" class="text-md text-gray-500"> {{ label }} </label>
    <textarea
      id="description"
      name="description"
      :placeholder="placeholder"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
      :class="[
        { 'border-2 border-red-500': error },
        'text-md text-gray-500 h-20 w-full bg-gray-25 md:bg-transparent border rounded-md mb-4 p-4 outline-none',
      ]"
    />
    <p class="text-red-500" v-if="error" data-test="textarea-input-text-error">
      {{ error }}
    </p>
  </div>
</template>
<script setup>
const props = defineProps({
  modelValue: {
    type: String,
    required: true,
  },
  error: {
    type: String,
    default: "",
  },
  label: {
    type: String,
    required: true,
  },
  placeholder: {
    type: String,
    default: "",
  },
});
</script>
