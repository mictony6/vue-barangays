<script setup>
import { computed, ref } from 'vue'

let province = ref(''),
  municipality = ref('')
let output = ref([])
function handleSubmit() {
  let url = `https://demo.myruntime.com/website/fulfillmentClustersService/api/getPhilClusterOptions/myruntimeWeb?parentOption=${province.value}&childOption=${municipality.value}`
  fetch(url)
    .then((res) => res.json())
    .then((data) => {
      output.value = data.data
    })
}
let options = ref(null)
const optionsUrl =
  'https://demo.myruntime.com/website/fulfillmentClustersService/api/getPhilClusters/myruntimeWeb'
fetch(optionsUrl)
  .then((res) => res.json())
  .then((data) => {
    // eslint-disable-next-line no-unused-vars
    options.value = data.data
  })
const provinces = computed(() => {
  return options.value ? options.value.parentOptions : ['No provinces found']
})

const municipalities = computed(() => {
  return province.value ? options.value.childOptions[province.value] : ['No municipalities found']
})

function handleReset() {
  province.value = ''
  municipality.value = ''
  output.value = []
}

const top = ref(null)
function scrollToTop() {
  top.value.scrollIntoView()
}

const items = ['#27586B', '#6D92A0', '#457485', '#103E50', '#022735']
function item() {
  return items[Math.floor(Math.random() * items.length)]
}
</script>
<template>
  <div class="container p-4">
    <div class="box">
      <div class="content">
        <h3 class="title is-4">Input</h3>
      </div>
      <form @submit.prevent="handleSubmit" @reset.prevent="handleReset">
        <div class="field">
          <label for="provinceInput" class="label">Province</label>
          <div class="control">
            <div class="select">
              <select name="province" id="provinceInput" v-model="province">
                <option value="" disabled selected>Select your province</option>
                <option v-for="(province, index) in provinces" :key="index">{{ province }}</option>
              </select>
            </div>
          </div>
        </div>
        <div class="field">
          <label for="municipalityInput" class="label">Municipality</label>
          <div class="control">
            <div class="select">
              <select v-model="municipality" name="municipality" id="municipalityInput">
                <option value="" disabled selected>Select your municipality</option>
                <option v-for="(municipality, index) in municipalities" :key="index">
                  {{ municipality }}
                </option>
              </select>
            </div>
          </div>
        </div>
        <div class="field is-grouped">
          <div class="control">
            <button type="submit" class="button is-link">Submit</button>
          </div>
          <div class="control">
            <button type="reset" class="button is-link is-light">Reset</button>
          </div>
        </div>
      </form>
    </div>
    <span ref="top"></span>

    <div class="box">
      <div class="content">
        <h3 class="title is-4">Output</h3>
        <p v-if="output.length" class="subtitle is-6">
          Barangays in <span class="is-underlined">{{ municipality }}, {{ province }}</span>
        </p>
      </div>
      <div v-if="!output.length" class="content has-text-centered has-text-grey">
        <p><small>Select a province and a municipality</small></p>
      </div>
      <div class="columns is-multiline">
        <div v-for="(barangay, index) of output" :key="index" class="column is-one-fifth">
          <p class="p-2 has-text-light" :style="{ background: item() }">{{ barangay }}</p>
        </div>
      </div>
    </div>

    <span id="to-top">
      <button class="button is-rounded is-info" @click.prevent="scrollToTop">Back to Top</button>
    </span>
  </div>
</template>

<style scoped>
#to-top {
  position: fixed;
  bottom: 2%;
  right: 1%;
}

.column > p {
  border-radius: 6px;
}
</style>
