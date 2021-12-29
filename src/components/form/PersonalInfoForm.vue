<template>
    <app-input
      id="name"
      name="name"
      type="text"
      label="Имя"
      v-model="name"
      :errors="v$.name.$errors"
      :required="true"
      classLabelBox="label__box-small"
    ></app-input>

    <app-input
      id="surname"
      name="surname"
      type="text"
      label="Фамилия"
      v-model="surname"
      :errors="v$.surname.$errors"
      :required="true"
    ></app-input>

    <app-input
      id="middleName"
      name="middleName"
      type="text"
      label="Отчество"
      v-model="middleName"
    ></app-input>

    <app-input
      id="dateOfBirth"
      name="dateOfBirth"
      type="date"
      label="Дата рождения"
      v-model="dateOfBirth"
      :errors="v$.dateOfBirth.$errors"
      classLabelBox="label__box-long"
      :required="true"
    >
    </app-input>

    <app-input
      id="phone"
      name="phone"
      type="tel"
      label="Номер телефона"
      v-model="phone"
      :errors="v$.phone.$errors"
      classLabelBox="label__box-longest"
      :required="true"
    >
      <div class="telNum">+7</div>
    </app-input>

    <InputRadio
      v-model="gender"
    />

    <MultiSelect
      :options="groupOptions"
      v-model="groupClient"
      form-name="Группа клиентов"
      :required="true"
      :errors="v$.groupClient.$errors"
      name="groupClient"
    />

    <AppSelect
      :options="doctorOptions"
      v-model="doctor"
      form-name="Лечащий врач"
      classes="select-pl-10"
      name="doctor"
    />


    <div class="form__control">
      <p class="form__name">Не отправлять смс</p>
      <input type="checkbox" class="input ml-10" name="sms" v-model="sms" @click="checked = !checked">
    </div>

    <button @click="checkValidation" class="btn btn__step">Следующий шаг</button>
</template>

<script>
import {computed, reactive, ref, toRefs} from "vue"
import AppInput from "../AppInput"
import InputRadio from "../form-element/InputRadio"
import MultiSelect from "../AppMultiSelect"
import AppSelect from "../AppSelect"
import {required, minLength, maxLength, helpers} from "@vuelidate/validators"
import useVuelidate from "@vuelidate/core"
import {requiredMessage} from "../../utils/errorConsts"

export default {
  emits: ['next'],
  setup(props, context) {
    const personalInfo = reactive({
      name: '',
      surname: '',
      middleName: '',
      dateOfBirth: '',
      gender: '',
      phone: '',
      groupClient: null,
      doctor: '',
      sms: false
    })

    const groupClientFc = (data) => {
      personalInfo.groupClient = data
    }

    const checked = ref(true)

    const groupOptions = [
      {
        value: 'vip',
        text: 'Vip'
      },
      {
        value: 'trouble',
        text: 'Проблемные'
      },
      {
        value: 'oms',
        text: 'Омс'
      }
    ]
    const doctorOptions = [
      {
        value: 'zaharov',
        text: 'Захаров'
      },
      {
        value: 'chernyshev',
        text: 'Чернышов'
      },
      {
        value: 'ivanov',
        text: 'Иванов'
      }
    ]

    const requiredRules = { required: helpers.withMessage(requiredMessage, required) }

    const rules = computed(() => {
      return {
        name: requiredRules,
        surname: requiredRules,
        dateOfBirth: requiredRules,
        phone: {
          required: helpers.withMessage(requiredMessage, required),
          minLength: helpers.withMessage('Поле должно быть длиннее 10 символов', minLength(10)),
          maxLength: helpers.withMessage('Поле должно быть короче 11 символов', maxLength(11)) },
        groupClient: requiredRules
      }
    })

    const v$ = useVuelidate(rules, personalInfo)

    const checkValidation = () => {
      v$.value.$validate()
      if (v$.value.$error) {
        return v$.value.$touch()
      }
      personalInfo.sms = checked.value
      context.emit('next', personalInfo)
    }

    return {
      ...toRefs(personalInfo),
      checkValidation,
      groupOptions,
      doctorOptions,
      checked,
      groupClientFc,
      v$,
    }
  },
  components: {
    AppInput,
    InputRadio,
    MultiSelect,
    AppSelect
  }
}

</script>
