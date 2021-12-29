<template>
  <app-input
    id="index"
    name="index"
    type="number"
    label="Индекс"
    v-model="index"
    classLabelBox="label__box-medium-60"
  ></app-input>

  <app-input
    id="country"
    name="country"
    type="text"
    label="Страна"
    v-model="country"
    classLabelBox="label__box-medium-60"
  ></app-input>

  <app-input
    id="region"
    name="region"
    type="text"
    label="Регион"
    v-model="region"
    classLabelBox="label__box-medium-60"
  ></app-input>

  <app-input
    id="city"
    name="city"
    type="text"
    label="Город"
    v-model="city"
    :errors="v$.city.$errors"
    classLabelBox="label__box-medium-55"
    :required="true"
  ></app-input>

  <app-input
    id="street"
    name="street"
    type="text"
    label="Улица"
    v-model="street"
    classLabelBox="label__box-medium-55"
  ></app-input>

  <app-input
    id="house"
    name="house"
    type="text"
    label="Дом"
    v-model="house"
    classLabelBox="label__box-small"
  ></app-input>

  <div class="form__buttons">
    <button class="btn btn__step" @click="backStep">Назад</button>
    <button class="btn btn__step" @click="nextStep">Следующий шаг</button>
  </div>
</template>

<script>
import {computed, reactive, toRefs} from "vue"
import {helpers, required} from "@vuelidate/validators"
import {requiredMessage} from "../../utils/errorConsts"
import useVuelidate from "@vuelidate/core"
import AppInput from "../AppInput"

export default {
  components: {AppInput},
  emits: ['next', 'back'],
  setup(props, context) {
    const addressInfo = reactive({
      index: '',
      country: '',
      region: '',
      city: '',
      street: '',
      house: ''
    })

    const requiredRules = { required: helpers.withMessage(requiredMessage, required) }

    const rules = computed(() => {
      return {
        city: requiredRules,
      }
    })

    const v$ = useVuelidate(rules, addressInfo)

    const backStep = () => {
      context.emit('back')
    }

    const nextStep = () => {
      v$.value.$validate()
      if (v$.value.$error) {
         return v$.value.$touch()
      }
      context.emit('next', addressInfo)
    }

   return {
    backStep,
    nextStep,
     ...toRefs(addressInfo),
    v$,
   }
  }
}
</script>

