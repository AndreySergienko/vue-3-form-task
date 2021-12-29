<template>
  <div class="form__control">
    <p class="form__name">Пол</p>
    <div class="form__items">
      <input type="radio" id="genderW" name="gender" class="input" value="woman" @input="changeValue" :value="modelValue">
      <label for="genderW" class="label-relative">Женский</label>

      <input type="radio" id="genderM" name="gender" class="input ml-10" value="man" @input="changeValue" :value="modelValue">
      <label for="genderM" class="label-relative">Мужской</label>
    </div>
  </div>
</template>

<script>
export default {
  emits: ['update:modelValue'],
  props: {
    modelValue: {
      type: String,
      required: false,
    },
  },
  methods: {
    changeValue(e) {
      return this.$emit('update:modelValue', e.target.value)
    }
  }
}
</script>

<style></style>
