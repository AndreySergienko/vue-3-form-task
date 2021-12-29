<template>
  <div class="form__control">
    <input
      :type="type"
      :id="id"
      class="input"
      placeholder=" "
      :name="name"
      :value="modelValue"
      @input="changeValue"
      :class="errors?.length ? 'error' : ''"
      :required="required"
    >
    <slot></slot>
    <div class="label__box" :class="classLabelBox"></div>
    <label :for="id" class="label">{{ label }}</label>
    <small v-for="err in errors" class="small small-error">* {{ err.$message }}</small>
    <small class="small small-required" v-if="required && !errors.length">* обязательное поле</small>
  </div>
</template>

<script>
export default {
  emits: [`update:modelValue`, 'update:blur'],
  props: {
    modelValue: {
      type: String,
      required: false,
    },
    classLabelBox: {
      type: String,
      required: false,
      default: ''
    },
    errors: {
      type: Array,
      required: false,
    },
    required: {
      type: Boolean,
      required: false,
      default: false
    },
    label: {
      type: String,
      required: true
    },
    name: {
      type: String,
      required: false
    },
    id: {
      type: String,
      required: true
    },
    type: {
      type: String,
      required: false,
      default: 'text',
    }
  },
  methods: {
    changeValue(e) {
      this.$emit('update:modelValue', e.target.value)
    }
  }
}
</script>
