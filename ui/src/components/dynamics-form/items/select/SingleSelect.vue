<template>
  <el-select
    filterable
    :teleported="true"
    popper-class="dynamics-single-select"
    clearable
    v-bind="$attrs"
    v-model="_modelValue"
  >
    <el-option
      v-for="(item, index) in option_list"
      :key="index"
      teleported
      :label="label(item)"
      :value="item[valueField]"
    >
    </el-option>
  </el-select>
</template>
<script setup lang="ts">
import type { FormField } from '@/components/dynamics-form/type'
import { computed, ref } from 'vue'
import _ from 'lodash'
const rowTemp = ref<any>()

const props = defineProps<{
  modelValue?: string
  formValue?: any
  formfieldList?: Array<FormField>
  field: string
  otherParams: any
  formField: FormField
  view?: boolean
}>()

const emit = defineEmits(['update:modelValue', 'change'])

const _modelValue = computed({
  get() {
    return props.modelValue
  },
  set(value) {
    emit('update:modelValue', value)
    emit('change', props.formField)
  }
})
const textField = computed(() => {
  return props.formField.text_field ? props.formField.text_field : 'key'
})

const valueField = computed(() => {
  return props.formField.value_field ? props.formField.value_field : 'value'
})

const option_list = computed(() => {
  return props.formField.option_list ? props.formField.option_list : []
})

const label = (option: any) => {
  //置空
  if (props.modelValue && option_list.value) {
    const oldItem = option_list.value.find((item) => item[valueField.value] === props.modelValue)
    if (!oldItem) {
      emit('update:modelValue', undefined)
    }
  }

  return option[textField.value]
}
</script>
<style lang="scss">
.dynamics-single-select {
  .el-select-dropdown {
    max-width: 1px;
  }
}
</style>
