<script 
  setup 
  lang="ts" 
  generic="TValue extends string | number | boolean  | null | undefined, TList = {[key: string]: any}"
>

import {
  Listbox,
  ListboxLabel,
  ListboxButton,
  ListboxOptions,
  ListboxOption,
} from '@headlessui/vue'
import { computed } from 'vue';

const props = defineProps<{
  modelValue: TValue
  label: string
  items: TList[]
  valueKey: keyof TList | ((o: TList) => Exclude<TValue, undefined>)
  labelKey: keyof TList | ((o: TList) => string)
}>()

defineEmits<{
  (e: 'update:modelValue', value: TValue): void
}>()

const valueFn = computed(() => typeof props.valueKey === 'function' ? props.valueKey : ((o: TList) => o[props.valueKey as keyof TList] as Exclude<TValue, undefined>))
const displayFn = computed(() => typeof props.labelKey === 'function' ? props.labelKey : ((o: TList) => o[props.labelKey as keyof TList]))

const selectedItem = computed(() => props.items.find(i => valueFn.value(i) === props.modelValue))
</script>

<template>
  <div class="w-72">
    <Listbox @update:model-value="$emit('update:model-value', $event)">
      <ListboxLabel>{{ label }}</ListboxLabel>
      <div class="relative">
        <ListboxButton class="relative w-full border text-left">
          <span class="block truncate">{{ selectedItem ? displayFn(selectedItem) : 'Please Select...' }}</span>
        </ListboxButton>
        <ListboxOptions class="absolute max-h-60 z-10 bg-white w-full overflow-auto py-1 border">
          <ListboxOption v-slot="{ active, selected }" v-for="item in items" :key="String(valueFn(item))"
            :value="valueFn(item)" as="template"
          >
            <li class="relative select-none" :class="{
              'bg-blue-100': active,
              'bg-blue-50': selected && !active
            }">
              <span>{{ displayFn(item) }}</span>
            </li>
          </ListboxOption>
        </ListboxOptions>
      </div>
    </Listbox>
  </div>
</template>