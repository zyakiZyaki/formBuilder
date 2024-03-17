<template>
  <div class="form-builder">
    <!-- Должно собираться автоматически, а не руками как сейчас, согласно конфигу   -->
    <h1>{{ type }}</h1>
    <div class="valid" v-if="valid === false">Ошибка валидации</div>
    <form ref="form">
      <form-input
        :type="config.inputType"
        :label="config.inputLabel"
        v-model="name"
      />
      <form-select
        :label="config.selectLabel"
        :options="config.selectOptions"
        v-model="select" 
      />
      <form-radio
        :label="config.radioLabel"
        :options="config.radioOptions"
        v-model="radio"
      />
      <form-password
        :label="config.passwordLabel"
        v-model="password"
      />
      <form-repeat-password
        :label="config.passwordRepeatLabel"
        v-model="repeatPassword"
      />
      <button type="submit" @click.prevent="onSubmit()">Отправить</button>
      <button type="reset">Стереть</button>
    </form>
  </div>
</template>

<script>
import FormInput from "@/components/form-items/FormInput.vue";
import FormSelect from "@/components/form-items/FormSelect.vue";
import FormRadio from "@/components/form-items/FormRadio.vue";
import FormPassword from "@/components/form-items/FormPassword.vue";
import FormRepeatPassword from "@/components/form-items/FormRepeatPassword.vue";

export default {
  name: "FormBuilder",
  props: {
    type: {
      type: String,
      default: 'parent'
    }
  },
  data() {
    return {
      name: '',
      select: '',
      radio: '',
      password: '',
      repeatPassword: '',
      config: {},
      valid: true
    }
  },

  async created() {

    // Создаем запрос

    const response = await fetch('./form-config.json')
    const data = await response.json()

    // Выбираем ветку по пропсу

    const path = await data[`${this.type}`]

    // Записываем данные

    this.config.inputType = path.items[0].type
    this.config.inputLabel = path.items[0].label
    this.config.selectLabel = path.items[1].label
    this.config.selectOptions = path.items[1].additional.options
    this.config.radioLabel = path.items[2].label
    this.config.radioOptions = path.items[2].additional.options
    this.config.passwordLabel = path.items[3].label
    this.config.passwordRepeatLabel = path.items[4].label
  },

  methods: {

    validation() {
      if (this.password !== this.repeatPassword
        || this.repeatPassword === ''
        || this.password === ''
        || this.select === ''
        || this.radio === '') {
        this.valid = false
        return false
      }
    },

    onSubmit() {
      if (this.validation() === false) {
        return
      }
      const user = {}
      user[`${this.type}`] = {
        name: this.name,
        gender: this.select,
        age: this.radio,
        pass: this.password,
      }

      fetch('https://foo.bar', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json;charset=utf-8'
        },
        body: JSON.stringify(user)
      })
      this.$refs.form.reset();
      alert('Данные отправлены!')
    }
  },
  components: { FormPassword, FormRadio, FormSelect, FormInput, FormRepeatPassword },
  watch: {
    name() {
      this.valid = true
    },
    select() {
      this.valid = true
    },
    password() {
      this.valid = true
    },
    radio() {
      this.valid = true
    },
    repeatPassword() {
      this.valid = true
    }
  }
}
</script>

<style scoped></style>
