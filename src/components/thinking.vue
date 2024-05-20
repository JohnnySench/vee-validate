<script setup>

import {Form, useForm} from "vee-validate";
import InputValidate from "@/components/inputValidate.vue";
import {computed, onMounted, ref, shallowRef} from "vue";
import PrimeCalendar from "primevue/calendar";
import PrimeInputText from "primevue/inputtext";
import PrimeDropdown from "primevue/dropdown";
import * as yup from "yup";

const schema = {
  desc: yup.string().required('Обязательное поле'),
}
const {defineField, errors, handleSubmit, setValues} = useForm({
  validationSchema: yup.object({...schema})
})
const response = ref('')
const event = ref({
  date: '',
  description: '123',
  locationId: ''
})
const eventFields = computed(() => {
  return [
    {
      vModel: event.value.date,
      name: 'date',
      label: 'Дата',
      settings: {
        showIcon: true,
        iconDisplay: 'input',
        class: 'w-16',
      },
      component: PrimeCalendar,
    },
    {
      vModel: event.value.description,
      name: 'description',
      label: 'Описание',
      settings: {

      },
      component: PrimeInputText,
    },
    {
      vModel: event.value.locationId,
      name: 'locationId',
      label: 'Место',
      settings: {
        options: response.value,
        optionLabel: "name",
        optionValue: 'id',
      },
      component: PrimeDropdown,
    },
  ]
})
const isLoading = shallowRef(true)
onMounted(() => {

  new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(
          response.value = [
            {name: 'string', id: '123'},
            {name: 'number', id: '124'},
            {name: 'boolean', id: '125'},
          ],
          isLoading.value = false
      )
    }, 3000)
  })
})

function setModelValue(e, fieldName) {
  if(!fieldName) return
  event.value[fieldName] = e;
}

const onSubmit = handleSubmit((values) => {
  console.log(JSON.stringify(values))
  console.log('rererer')
})
</script>

<template>
  <div v-if="!isLoading">
    <form @submit="onSubmit">
      <div class="fields" v-for="field in eventFields">
        <input-validate
            v-model="field.vModel"
            :current-component="field.component"
            :name="field.name"
            :label="field.label"
            v-bind="field.settings"
            @update:model-value="(e, fieldName) => setModelValue(e, fieldName)"
        />
      </div>
      <button type="submit">Check</button>
    </form>
  </div>
</template>

<style scoped>

</style>