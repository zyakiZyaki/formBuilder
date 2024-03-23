<template>
  <div class="form-builder">
    <div class="valid" v-if="valid === false">Ошибка валидации</div>

    <!-- Должно собираться автоматически, а не руками как сейчас, согласно конфигу   -->

    <form v-for="form in this.forms" @submit.prevent="onSubmit" :ref="form" :key="form">

      <h1>{{ form }}</h1>

      <component
        v-for="(component, index) in this.config[`${form}`].items"
        :type="component.type"
        :label="component.label" :options="component.additional?.options" :key="index"
        :is="normalizeFormName(component.type)" @modelValue="data[form][component.name] = $event"
      />

      <button type="submit">Отправить</button>
      <button type="reset">Стереть</button>

    </form>
  </div>
</template>

<script>
import FormInput from "@/components/form-items/FormInput.vue";
import FormSelect from "@/components/form-items/FormSelect.vue";
import FormRadio from "@/components/form-items/FormRadio.vue";
import FormPassword from "@/components/form-items/FormPassword.vue";

export default {
  name: "FormBuilder",
  data() {
    return {
      data: {},
      config: {},
      valid: true
    }
  },

  async created() {

    // Запрос на сервер с файлом конфига

    const response = await fetch('./form-config.json')
    const data = await response.json()

    // Копируем, чтобы удобнее работать с this

    this.config = { ...data }

    // Подготавливаем каркас формы отправки данных

    this.addFormsToData()

  },

  computed: {
    
    // Получаем ключи из конфига - название форм

    forms() {
      return Object.keys(this.config)
    },
  },

  methods: {

    // Cоздаем каркас формы отправки

    addFormsToData() {
      this.forms.forEach(item => {
        this.data[item] = {}
      })
    },

    //Динамически называем компонент

    normalizeFormName(name) {
      return 'Form' + name[0].toUpperCase() + name.slice(1)
    },

    validation() {

    // Выбираем ветку и валидируем

      for (const form of Object.values(this.data)) {
        if (Object.keys(form).length === 0
          || form['pass'] === undefined
          || form['repeat-pass'] === undefined) {
          this.valid = false
          return false
        }
        if (form['pass'] !== form['repeat-pass']) {
          this.valid = false
          return false
        }

    // Удаляем из формы repeat-pass

        else {
          delete form['repeat-pass']
        }
      }
    },

    onSubmit() {

    // Валидация

      if (this.validation() === false) {
        return
      }

    // Отправка запроса

      fetch('https://foo.bar', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json;charset=utf-8'
        },
        body: JSON.stringify(this.data)
      })
      alert('Данные отправлены!')
    }
  },

  watch: {

    //При глубоком изменении data сбрасываем валидацию

    data: {
      handler() {
        this.valid = true
      },
      deep: true
    }
  },
  components: { FormPassword, FormRadio, FormSelect, FormInput },
}

</script>

<style scoped></style>
