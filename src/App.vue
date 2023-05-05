<script setup lang="ts">
import VSelect from '@/components/VSelect.vue'
import { ref } from 'vue'

interface Person {
  id: number
  name: string
}

interface Car {
  id: number
  make: string
  model: string
}

const people: Person[] = [
  {
    id: 1,
    name: 'Bilbo'
  },
  {
    id: 2,
    name: 'Gandalf'
  },
  {
    id: 3,
    name: 'Frodo'
  }
]

const cars: Car[] = [
  {
    id: 1,
    make: 'Ford',
    model: 'Fiesta',
  },
  {
    id: 2,
    make: 'Citroën',
    model: 'Saxo'
  },
  {
    id: 3,
    make: 'Nissan',
    model: 'Leaf'
  },
]

const person = ref<number>()
const car = ref<number>()

const invalidPerson = ref<number>()
const invalidCar = ref<number>()
</script>

<template>
  <h2 class="text-lg font-bold">Valid</h2>
  <span>Person Value: {{ person }}</span>
  <VSelect label="Person" v-model="person" :items="people" :value-key="p => p.id" label-key="name" />
  <span>Car Value: {{ car }}</span>
  <VSelect label="Car" v-model="car" :items="cars" :value-key="c => c.id" :label-key="c => `${c.make} ${c.model}`" />

  <h2 class="text-lg font-bold mt-6">Type Errors</h2>
  <span>Person Value: {{ invalidPerson }}</span>
  <VSelect label="Person" v-model="invalidPerson" :items="people" :value-key="p => p.invalidKey" label-key="invalidKey" />
  <span>Car Value: {{ invalidCar }}</span>
  <VSelect label="Car" v-model="invalidCar" :items="cars" :value-key="c => c.car_id" :label-key="c => `${c.year} ${c.model}`" />
</template>
