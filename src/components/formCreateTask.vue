<template>
  <div class="flex column flex-center q-px-lg bg-amber-1">
    <h3>
      Создать Задачу
    </h3>

    <q-form
        class="q-gutter-md"
    >
      <q-input
          filled
          label="Имя"
          hint="Имя"
          name="name"
          v-model="form_name"
      />


      <q-input
          filled
          label="Телефон"
          hint="+7(812)123-35-98"
          v-model="form_phone"
      />
      <q-input
          filled
          label="E-mail"
          type="email"
          hint="Name and surname"
          v-model="form_email"
      />
      <q-input
          filled
          label="Цены"
          hint="300$"
          v-model="form_price"
      />
      <q-input
          filled
          label="Дата"
          hint="01.01.2003"
          type="date"
          v-model="form_date"
      />
      <q-input
          filled
          label="Тип оплаты"
          hint="(Наличные безнал)"
          v-model="form_pay"
      />
      <div>
        <q-btn label="Submit" type="submit" @click.prevent="sendForm(form)" color="primary"/>
        <q-btn label="Reset" type="reset" @click="resetForm" color="primary"/>
      </div>
    </q-form>
  </div>
</template>

<script setup>
import {computed, ref} from "vue";
import {useQuasar} from 'quasar'

const $q = useQuasar()
let form_name = ref('')
let form_phone = ref('')
let form_email = ref('')
let form_price = ref('')
let form_date = ref('')
let form_pay = ref('')
const props = defineProps({
  modelValue: {
    type: Object,
  }
});
let form = {
  name: form_name,
  phone: form_phone,
  email: form_email,
  price: form_price,
  pay: form_pay,
  date: form_date
}

const emit = defineEmits(['modelValue']);
const localComputed = computed({
  get() {
    return form;
  },
  set() {
    emit('modelValue', form)
  }
});
const sendForm = (form) => {
  emit('modelValue', localComputed)
  resetForm()
}
const resetForm = () => {
  form_name.value = ''
  form_phone.value = ''
  form_email.value = ''
  form_price.value = ''
  form_date.value = ''
  form_pay.value = ''
}
</script>

<style>
.form_wrap {
  background: #000;
}

</style>
