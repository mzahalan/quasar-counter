<template>
  <q-page
    class="flex flex-center text-white"
    v-touch-pan.vertical.prevent.mouse="handlePan"
    >
    <div class="row">
      <q-input
        v-model="data.name"
        input-class="text-center text-h5 text-white"
        color="teal"
        placeholder="Counter"
        filled
      />
    </div>
    <div class="row full-width items-center">
      <div class="col text-center">
        <q-btn
          round
          size="xl"
          icon="remove"
          @click="decreaseCounter"
          v-touch-repeat:300:300:300:300:50.mouse="decreaseCounter"/>
      </div>
      <div class="col text-center text-h2">{{ data.counter }}</div>
      <div class="col text-center">
        <q-btn
          round
          size="xl"
          icon="add"
          @click="increaseCounter"
          v-touch-repeat:300:300:300:300:50.mouse="increaseCounter"/>
      </div>
    </div>
    <div class="row">
      <q-btn
        @click="resetCounter"
        icon="restart_alt"
        size="xl"
        round
      />
    </div>
  </q-page>
</template>

<script setup>
import {reactive, watch} from 'vue'
import {useQuasar} from 'quasar'

const $q = useQuasar()

const data = reactive({counter: 0, name: ''})
const savedData = $q.localStorage.getItem('data')
if(savedData) {
  Object.assign(data, savedData)
}
watch(data, value => {
  $q.localStorage.set('data', value)
})



const increaseCounter = () => {
  data.counter++
}
const decreaseCounter = () => {
  if(data.counter > 0) {
    data.counter--
  }
}

const resetCounter = () => {
  data.counter = 0
}

const handlePan = (e) => {
  if(e.delta.y < 0) {
    increaseCounter()
  } else {
    decreaseCounter()
  }
}

</script>

<style scoped>
.q-page {
  max-width: 700px;
  margin: 0 auto;
}
</style>
