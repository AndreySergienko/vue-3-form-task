<template>
  <transition name="alert">
    <div class="alert" v-show="alert" @click="alert = false">
      Аккаунт успешно создан
    </div>
  </transition>
  <div class="form">
    <h1 class="form__title">Регистрация</h1>

    <p class="form__subtitle">{{ subtitleName }}</p>
    <div class="form__steps">{{ step }} / 3</div>

    <keep-alive v-if="someState">
      <PersonalInfoForm
        v-if="step === 1"
        @next="nextStep"
      />
    </keep-alive>

    <keep-alive v-if="someState">
      <AddressInfoForm
        v-if="step === 2"
        @next="nextStep"
        @back="backStep"
      />
    </keep-alive>

    <keep-alive v-if="someState">
      <PassportInfoForm
        v-if="step === 3"
        :info="info"
        @create="create"
        @back="backStep"
      />
    </keep-alive>

  </div>
</template>

<script>
import {computed, nextTick, ref} from "vue"
import PersonalInfoForm from "./components/form/PersonalInfoForm"
import AddressInfoForm from "./components/form/AddressInfoForm"
import PassportInfoForm from "./components/form/PassportInfoForm"

export default {
  setup() {
    const step = ref(1  )
    const info = ref([])
    const alert = ref(false)
    const someState = ref(true)

    const create = () => {
      alert.value = true
      step.value = 1
      nextTick(() => reloadComponent())
      setTimeout(() => {
        alert.value = false
      }, 5000)
    }

    const reloadComponent = () => {
      someState.value = false
      setTimeout(() => {
        someState.value = true
      }, 1)
    }

    const nextStep = (data) => {
      info.value.push(data)
      return step.value++
    }

    const backStep = () => {
      return step.value--
    }

    const subtitleName = computed(() => {
      switch (step.value) {
        case 1:
          return 'Персональная информация'
        case 2:
          return 'Адрес'
        case 3:
          return 'Паспортные данные'
      }
    })

    return {
      step,
      subtitleName,
      create,
      nextStep,
      info,
      backStep,
      someState,
      alert
    }
  },
  components: {
    PassportInfoForm,
    AddressInfoForm,
    PersonalInfoForm,
  }
}
</script>

<style lang="scss" scoped>
.alert {
  position: absolute;
  z-index: 2;
  padding: 20px;
  width: 300px;
  background: #fff;
  border-left: 5px solid #00c000;
  border-radius: 5px;
}

.alert-enter-from,
.alert-leave-to {
  transform: translateY(700px);
  opacity: 0;
}

.alert-enter-active {
  transition: all 0.3s ease-out;
}

.alert-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

</style>
