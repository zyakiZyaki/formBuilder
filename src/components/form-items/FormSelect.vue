<template>
  <div>
    <p>{{ label }}</p>
    <select
    @change="$emit('update:modelValue', $event.target.value)" >
      <option
      v-for="option in options"
      :key="option.value"
      :selected="option.selected">
      {{ option.text }}</option>
    </select>
  </div>
</template>

<script>

import { mapMutations } from 'vuex';

export default {
  name: "FormSelect",
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
