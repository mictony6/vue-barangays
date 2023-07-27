<template>
  <div class="container p-4">
    <div class="box">
      <div class="content">
        <h3 class="title is-4">Input</h3>
      </div>

      <form @submit.prevent="handleSubmit" @reset.prevent="handleReset" class="px-4">
        <div class="field">
          <label for="provinceInput" class="label">Province</label>
          <div class="control has-icons-left">
            <span class="icon is-left is-small">
              <img :src="pinIcon" alt="" />
            </span>
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
          <div class="control has-icons-left">
            <span class="icon is-left is-small">
              <img :src="pinIcon" alt="" />
            </span>
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

    <OutputBox
      @sortAscending="handleSort()"
      @sortDescending="handleSort(true)"
      :barangays="output"
      :municipality="municipality"
      :province="province"
    />
  </div>
</template>

<script>
import pinIcon from '@/assets/map-pin.svg'
import OutputBox from '@/components/OutputBox.vue'
import { ref } from 'vue'

export default {
  components: { OutputBox },
  setup() {
    let options = ref([])
    const optionsUrl =
      'https://demo.myruntime.com/website/fulfillmentClustersService/api/getPhilClusters/myruntimeWeb'
    //fetches options for provinces and municipalities
    fetch(optionsUrl)
      .then((res) => res.json())
      .then((data) => {
        options.value = data.data
      })
    return {
      optionsUrl,
      options,
      pinIcon
    }
  },
  data() {
    return {
      output: [],
      province: '',
      municipality: ''
    }
  },
  computed: {
    // options for province
    provinces() {
      return this.options ? this.options['parentOptions'] : ['No provinces found']
    },
    // options for municipality
    municipalities() {
      return this.province
        ? [...this.options['childOptions'][this.province]].sort()
        : ['No municipalities found']
    }
  },
  methods: {
    // runs when submit button was clicked
    handleSubmit() {
      let url = `https://demo.myruntime.com/website/fulfillmentClustersService/api/getPhilClusterOptions/myruntimeWeb?parentOption=${this.province}&childOption=${this.municipality}`
      fetch(url)
        .then((res) => res.json())
        .then((data) => {
          this.output = data.data
        })
    },
    // resets form input values
    handleReset() {
      this.province = ''
      this.municipality = ''
      this.output = []
    },
    handleSort(descending = false) {
      descending ? this.output.sort().reverse() : this.output.sort()
    }
  }
}
</script>

<style scoped></style>
