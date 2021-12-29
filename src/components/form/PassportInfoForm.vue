<template>
  <AppSelect
    form-name="Тип документа"
    v-model="typeDocument"
    name="typeDocument"
    :options="documentOptions"
    classes="select-pl-10"
    :required="true"
    :errors="v$.typeDocument.$errors"
  />

  <app-input
    id="seriesPassport"
    name="seriesPassport"
    type="number"
    label="Серия"
    v-model="seriesPassport"
    classLabelBox="label__box-medium-55"
  ></app-input>

  <app-input
    id="numberPassport"
    name="numberPassport"
    type="number"
    label="Номер"
    v-model="numberPassport"
    classLabelBox="label__box-medium-65"
  ></app-input>

  <app-input
    id="givenPassport"
    name="givenPassport"
    type="text"
    label="Кем выдан"
    v-model="givenPassport"
    classLabelBox="label__box-medium-85"
  ></app-input>

  <app-input
    id="dateGiven"
    name="dateGiven"
    type="date"
    label="Дата выдачи"
    v-model="dateGiven"
    :errors="v$.dateGiven.$errors"
    classLabelBox="label__box-medium-100"
    :required="true"
  >
  </app-input>

  <div class="form__buttons">
    <button class="btn btn__step" @click="backStep">Назад</button>
    <button class="btn btn__step" @click="createTicket">Создать аккаунт</button>
  </div>

</template>

<script>
import {computed, reactive, toRefs} from "vue"
import {helpers, required} from "@vuelidate/validators"
import {requiredMessage} from "../../utils/errorConsts"
import useVuelidate from "@vuelidate/core"
import AppSelect from "../AppSelect"
import AppInput from "../AppInput"

export default {
  emits: ['back', 'create'],
  props: {
    info: {
      type: Array,
      required: true
    }
  },
  setup(props, context) {
    const passportInfo = reactive({
      typeDocument: 'passport',
      seriesPassport: '',
      numberPassport: '',
      givenPassport: '',
      dateGiven: ''
    })

    const documentOptions = [
      {
        value: 'passport',
        text: 'Паспорт'
      },
      {
        value: 'certificate',
        text: 'Свидетельство о рождении'
      },
      {
        value: 'license',
        text: 'Водительское удостоверение'
      }
    ]

    const backStep = () => {
      context.emit('back')
    }

    const createTicket = () => {
      v$.value.$validate()
      if (v$.value.$error) {
        return v$.value.$touch()
      }

      const dto = {
        personInfo: props.info[0],
        addressInfo: props.info[1],
        passportInfo
      }
      console.log(dto)
      context.emit('create', true)
    }

    const requiredRules = { required: helpers.withMessage(requiredMessage, required) }

    const rules = computed(() => {
      return {
        typeDocument: requiredRules,
        dateGiven: requiredRules,
      }
    })

    const v$ = useVuelidate(rules, passportInfo)

    return {
      ...toRefs(passportInfo),
      v$,
      backStep,
      createTicket,
      documentOptions
    }

  },
  components: {
    AppSelect,
    AppInput
  }
}
</script>
