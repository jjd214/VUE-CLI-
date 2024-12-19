<script setup>
import { ref, nextTick, reactive, computed } from 'vue'

// Ref
const counter = ref(0)

const incrementHandler = () => {
  counter.value++
}

const person = ref({
  name: 'John Jacob',
  age: 21,
  favFoods: [
    {
      name: 'Adobo',
      purchase: true,
    },
    {
      name: 'Sinigang',
      purchase: false,
    },
    {
      name: 'Menudo',
      purchase: true,
    },
  ],
  contacts: [
    {
      name: 'Marco Gador',
      number: '09394331559',
    },
    {
      name: 'Jancis Discarga',
      number: '09605656273',
    },
  ],
})

const isFoodAdded = ref(false)

const addFoodHandler = () => {
  person.value.favFoods.push({ name: 'BBQ', purchase: false })
  isFoodAdded.value = true
}

// Reactive
const state = reactive({ tap: 0 })

const resetHandler = () => {
  counter.value = 0
  state.tap = 0

  // Next Tick
  nextTick(() => {
    console.log('nextTick')
  })
}

// Computed
const user = reactive({
  name: 'John Jacob',
  age: 21,
})

const userCheck = computed(() => {
  return user.age > 18 ? user.name + ' is qualifed' : user.name + ' is not qualifed'
})

// Writable computed
const fname = ref('Jancis')
const lname = ref('Discarga')
const fullName = computed({
  get() {
    return fname.value + ' ' + lname.value
  },
  set(newValue) {
    ;[fname, lname] = newValue.split(' ')
  },
})

const prevCounter = computed((previous) => {
  if (counter.value < 6) return counter.value
  return previous
})

const disabledButton = ref(true)

// Class binding
const status = ref('text-danger')

const purchaseHandler = (index) => {
  person.value.favFoods[index].purchase = !person.value.favFoods[index].purchase
}
</script>

<template>
  <main>
    <h1>Count : {{ counter }}</h1>
    <span>Previous counter: {{ prevCounter }}</span>
    <button @click="incrementHandler">Increment</button>

    <h1>Name: {{ person.name }}</h1>
    <h3>Favorite foods</h3>
    <ul>
      <li
        v-for="(food, index) in person.favFoods"
        :key="index"
        :class="food.purchase ? 'purchase' : 'selled'"
        @click="purchaseHandler(index)"
      >
        {{ food.name }}
      </li>
    </ul>

    <button @click="addFoodHandler" :disabled="isFoodAdded">Click me to add food</button>

    <h1>Taps: {{ state.tap }}</h1>
    <button @click="state.tap++">Tap</button>
    <button @click="resetHandler">Reset taps / counts</button>

    <h1>User {{ user.name }}</h1>
    <span>{{ userCheck }}</span>

    <h1>Some random name: {{ fullName }}</h1>
    <span>Writable computed</span><br />

    <span :v-show="false">Show me!</span>
    <button :disabled="disabledButton">Disabled Button</button>

    <h1 class="underline">Class binding</h1>
    <h1 class="underline" :class="status">Status</h1>
  </main>
</template>

<style scope>
.underline {
  text-decoration: underline;
}
.purchase {
  text-decoration: line-through;
  color: red;
}

.selled {
  font: italic;
  color: blue;
}
</style>
