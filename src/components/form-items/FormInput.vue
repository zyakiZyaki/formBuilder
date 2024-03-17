<template>
  <div>
    <p>{{ label }}</p>
    <input
    @input="$emit('update:modelValue', $event.target.value)"
    v-bind="$attrs"
    :type="type"
    required />
  </div>
</template>

<script>
export default {
  name: "FormInput",
  props: {
    label: {
      type: String,
      default: ''
    },
    type: {
      type: String,
      default: 'text'
    }
  }
}
</script>

<style scoped></style>
