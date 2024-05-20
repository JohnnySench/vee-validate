<script setup>
import {computed, onMounted, ref, shallowRef, toRefs} from "vue";
import PrimeInputText from "primevue/inputtext"
import PrimeFloatLabel from "primevue/floatlabel"
import {useForm, Form} from "vee-validate";
import * as yup from 'yup';
import NestedValidate from "@/components/nestedValidate.vue";
import InputValidate from "@/components/inputValidate.vue";
import PrimeCalendar from 'primevue/calendar';
import PrimeListbox from 'primevue/listbox';
import PrimeDropdown from 'primevue/dropdown';
import InputValidateWork from "@/components/inputValidateWork.vue";


const {defineField, errors, handleSubmit, setValues} = useForm({
  validationSchema: yup.object({
    name: yup.string().required('Обязательное поле'),
    test: yup.string().required('Обязательное поле'),
    password: yup.string().required('Обязательное поле'),
    numberUser: yup.string().required('Обязательное поле'),
    firstName: yup.string().required('Обязательное поле'),
    'event.value.description': yup.string().required('Обязательное поле'),
    description: yup.string().required('Обязательное поле'),
    select: yup.string().required('Обязательное поле'),
  }),
})


const payload = ref({
  numberUser: '',
  firstName: '',
})

const [numberUser] = defineField('payload.value.number')
const [firstName] = defineField('payload.value.firstName')

const smallFields = computed(() => {
  return [
    {
      vModel: numberUser,
      label: 'Номер',
      component: PrimeInputText,
      name: 'number',
      settings: {}
    },
    {
      vModel: firstName,
      label: 'Имя',
      component: PrimeInputText,
      name: 'firstName',
      settings: {}
    },
  ]
})


const [name, nameAttrs] = defineField('name')
const [password] = defineField('password')
const [test, testAttrs] = defineField('test')


const fields = computed(() => {
  return [
    {
      label: 'Имя', vModel: name, component: PrimeInputText, name: 'name', settings: {
        placeholder: 'opa',
      }
    },
    {label: 'Пароль', vModel: password, component: PrimeInputText, name: 'password', settings: {}},
  ]
})

const onSubmit = handleSubmit((values) => {
  console.log(1000, values)
  console.log(JSON.stringify(values))
  console.log('rererer')
})


const modelTest = ref('')

const place = ref()
const isLoading = shallowRef(true)
const [description, attrs1] = defineField('description')
const [select, attrs2] = defineField('select')

const data = ref({
  description,
  select,
})

const workFields = computed(() => {
  return [
    {
      vModel: description,
      component: PrimeInputText,
      label: 'Text',
      name: 'description',
      settings: {}
    },
    {
      vModel: select,
      component: PrimeDropdown,
      label: 'Drop',
      name: 'location',
      settings: {
        options: place.value,
        optionLabel: 'name',
        optionValue: 'value'
      }
    },
  ]
})

onMounted(() => {
  new Promise((resolve, reject) => {
    setTimeout(() => {
      place.value = [
        {name: 'string', value: '123'},
        {name: 'number', value: '124'},
        {name: 'boolean', value: '125'},
      ]
      isLoading.value = false

      console.log(data.value)
    }, 3000)
  })
})


</script>

<template>
  <div class="container p-10 grid grid-cols-1 gap-6">
    <form @submit="onSubmit" class="grid grid-cols-1 gap-6">
      <div class="field" v-for="field in fields" :key="field.name">
        <prime-float-label class="w-16">
          <component
              :is="field.component"
              :id="field.name"
              v-model="field.vModel.value"
              v-bind="field.settings"
          />
          <label
              :for="field.name"
              :class="{'text-red-500' : errors[field.name]}"
              v-html="'label'"
          />
        </prime-float-label>
        <small
            class="text-red-500"
            v-html="errors[field.name]"
        />
      </div>

      <div class="field">
        <prime-float-label class="w-16">
          <prime-input-text v-bind="testAttrs" id="test" v-model="test"></prime-input-text>
          <label for="test" v-html="'test'" :class="{'text-red-500' : errors.test}"/>
        </prime-float-label>
      </div>
      <button type="submit">Отправить</button>
    </form>

  </div>
  <nested-validate/>


  <div v-if="!isLoading">
    <form @submit="onSubmit">
      <div class="fields" v-for="field in workFields" :key="field.name">
        <input-validate-work
            v-model="field.vModel.value"
            :current-component="field.component"
            :name="field.name"
            :label="field.label"
            v-bind="field.settings"
        />
      </div>
      <button type="submit">Cehck</button>
    </form>
  </div>


</template>

<style scoped>

</style>
