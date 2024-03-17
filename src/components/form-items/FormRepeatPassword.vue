<template>
  <FormInput
  :label="label"
  type="password"
  @change="$emit('update:modelValue', $event.target.value)" />
</template>

<script>

import FormInput from "@/components/form-items/FormInput.vue";

import { mapMutations } from "vuex";

export default {
  name: "FormRepeatPassword",
  props: {
    label: {
      type: String,
      default: ''
    }
  },
  components: {
    FormInput
  }
}
</script>

<style lang="scss" scoped></style>
