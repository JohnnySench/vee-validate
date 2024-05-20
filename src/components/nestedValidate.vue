<script setup>
import {computed, onMounted, ref} from "vue";
import PrimeInputText from "primevue/inputtext"
import PrimeFloatLabel from "primevue/floatlabel"
import {useForm, Form, useField} from "vee-validate";
import * as yup from 'yup';

onMounted(() => {
  setTimeout(() => {
    const response = 'string'
    setValues({
      numberUser: response
    })
  }, 3000)
})

const payload = ref({
  numberUser: '123',
  firstName: '',
})
const {defineField, errors, handleSubmit, setValues} = useForm({
  validationSchema: yup.object({
    numberUser: yup.string().required('Обязательное поле'),
    firstName: yup.string().required('Обязательное поле'),
  }),
  initialValues: {
    numberUser: payload.value.numberUser,
    firstNameUser: payload.value.firstName,
  }
})



const [numberUser] = defineField('numberUser')
const [firstNameUser] = defineField('firstNameUser')

const smallFields = computed(() => {
  return [
    {
      vModel: numberUser,
      label: 'Номер',
      component: PrimeInputText,
      name: 'number',
      settings: {},
      refName: 'numberUser'
    },
    {
      vModel: firstNameUser,
      label: 'Имя',
      component: PrimeInputText,
      name: 'name',
      settings: {},
      refName: 'firstName'
    },
  ]
})

function logger(e, field) {
  console.log(1)
  payload.value[field] = e;
}


const onSubmit = handleSubmit((values) => {
  console.log(JSON.stringify(values))
  console.log('rererer')
})

</script>

<template>
  <div class="container p-10 grid grid-cols-1 gap-6">
    <div>{{payload.numberUser}} payload</div>
    <div>{{numberUser}} number</div>
    <form @submit="onSubmit" class="grid grid-cols-1 gap-6">
      <div class="field" v-for="(field, index) in smallFields">
        <prime-float-label class="w-16">
          <component
              :is="field.component"
              :id="field.name"
              v-model="field.vModel.value"
              v-bind="field.settings"
              @update:modelValue="(e) => logger(e, field.refName)"
          />
          <label
              :for="field.name"
              :class="{'text-red-500' : errors[field.name]}"
              v-html="field.label"
          />
        </prime-float-label>
        <small
            class="text-red-500"
            v-html="errors[field.name]"
        />
      </div>
      <button type="submit">Отправить</button>
    </form>

  </div>
</template>

<style scoped>

</style>
