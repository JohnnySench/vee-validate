<script setup lang="ts">
import {useAttrs} from "vue";
import PrimeFloatLabel from "primevue/floatlabel"
import {useField} from "vee-validate";

interface IProps {
  currentComponent: any,
  name: string,
  label: string,
}

const attrs = useAttrs()
const props = defineProps<IProps>()
const {value: inputValue, errors} = useField(() => props.name, undefined, {
  syncVModel: true,
});


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
      />
      <label :for="name" v-html="label" :class="{'text-red-500' : !!errors[0]}"/>
    </prime-float-label>
    <small v-html="errors[0]" class="text-red-500"/>
  </div>
</template>

<style scoped>

</style>