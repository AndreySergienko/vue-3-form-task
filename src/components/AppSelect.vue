<template>
  <div class="form__control">
    <p class="form__name">{{ formName }}</p>
    <div class="select-border">
      <select :name="name" class="select" :class="classes" @input="changeValue">
        <option
          v-for="option in options"
          :value="options.value"
          class="options"
        >{{ option.text }}</option>
      </select>
    </div>
    <small class="small small-required" v-if="required && !errors.length">* обязательное поле</small>
    <small class="small small-error" v-for="error in errors">* {{ error.$message }}</small>
  </div>
</template>

<script>
export default {
  emits: ['update:modelValue'],
  props: {
    formName: {
      type: String,
      required: true
    },
    errors: {
      type: Array,
      required: false,
      default: []
    },
    options: {
      type: Array,
      required: true
    },
    required: {
      type: Boolean,
      required: false,
      default: false
    },
    name: {
      type: String,
      required: true
    },
    classes: {
      type: String,
      required: false,
      default: ''
    }
  },
  methods: {
    changeValue(e) {
      this.$emit('update:modelValue', e.target.value)
    }
  }
}
</script>
