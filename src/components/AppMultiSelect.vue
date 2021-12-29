<template>
  <div class="form__control">
    <p class="form__name">{{ formName }}</p>
    <select multiple="multiple" :size="size" :name="name" class="select" @change="changeValue">
      <option
        v-for="option in options"
        :value="options.value"
        class="options"
      >{{ option.text }}</option>
    </select>
    <small class="small small-required" v-if="required && !errors.length">* обязательное поле</small>
    <small class="small small-error" v-for="error in errors">* {{ error.$message }}</small>
  </div>
</template>

<script>
import {ref} from "vue"

export default {
  emits: ['update:modelValue'],
  props: {
    formName: {
      type: String,
      required: true
    },
    size: {
      type: Number,
      required: false,
      default: 3
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
    errors: {
      type: Array,
      required: false,
      default: []
    },
    name: {
      type: String,
      required: true
    }
  },
  setup(_, context) {
    const arr = ref([])

    const changeValue = (e) => {
      if (arr.value.find(i => i === e.target.value)) {
        const idx = arr.value.findIndex(i => i === e.target.value)
        arr.value.splice(idx, 1)
      } else {
        arr.value.push(e.target.value)
      }

      context.emit('update:modelValue', arr.value)
    }

    return {
      changeValue,
    }
  }
}
</script>

