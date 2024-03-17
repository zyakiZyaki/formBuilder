<template>
  <fieldset>
    <legend>{{ label }}</legend>

    <div
    v-for="option in options"
    :key="option.value">
      <input
      @change="$emit('update:modelValue', $event.target.value)"
      :id="option.value"
      :value="option.value"
      :checked="option.selected"
      type="radio" name="radio" required />
      <label :for="option.value">{{ option.text }}</label>
    </div>
  </fieldset>
</template>

<script>

export default {
  name: "FormRadio",

  props: {
    label: {
      type: String,
      default: ''
    },

    options: {
      type: Object,
      required: true,
      default: []
    }
  },
}
</script>

<style scoped></style>
