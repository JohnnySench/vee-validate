<script setup lang="ts">
import {computed, onMounted, ref, useAttrs} from "vue";
import PrimeInputText from "primevue/inputtext"
import PrimeFloatLabel from "primevue/floatlabel"
import {useForm, Form, useField} from "vee-validate";
import * as yup from 'yup';

interface IProps {
  currentComponent: any,
  name: string,
  label: string,
  modelValue: string,
}

const attrs = useAttrs()
const props = defineProps<IProps>()
const {value: inputValue, errors} = useField(() => props.name, undefined, {
  syncVModel: true,
});

interface IEmits {
  (e: 'update:modelValue', data: any, fieldName: string)
}

const emits = defineEmits<IEmits>()

function update(e) {
  emits('update:modelValue', e, props.name)
}


</script>

<template>
  <div class="field">
    <prime-float-label class="w-16">
      <component
          :is="currentComponent"
          :id="name"
          v-model="inputValue"
          :invalid="!!errors[0]"
          v-bind="attrs"
          @update:modelValue="(e) => update(e)"
      />
      <label :for="name" v-html="label" :class="{'text-red-500' : !!errors[0]}"/>
    </prime-float-label>
    <small v-html="errors[0]" class="text-red-500"/>
  </div>
</template>

<style scoped>

</style>