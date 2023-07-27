<template>
  <span ref="top"></span>

  <div class="box">
    <div class="content">
      <h3 class="title is-4">Output</h3>
      <p v-if="barangays.length" class="subtitle is-6">
        Barangays in <span class="is-underlined">{{ municipality }}, {{ province }}</span>
      </p>
      <div class="buttons">
        <button class="button" @click.prevent="$emit('sortAscending')">Sort A-z</button>
        <button class="button" @click.prevent="$emit('sortDescending')">Sort z-A</button>
      </div>
    </div>
    <div v-if="!barangays.length" class="content has-text-centered has-text-grey">
      <p><small>Select a province and a municipality</small></p>
    </div>
    <div class="columns is-multiline px-4">
      <div v-for="(barangay, index) of barangays" :key="index" class="column is-one-fifth">
        <p class="barangay p-2 has-text-light">{{ barangay }}</p>
      </div>
    </div>
  </div>

  <span id="to-top">
    <button class="button is-rounded is-info" @click.prevent="scrollToTop">Back to Top</button>
  </span>
</template>

<script>
import { ref } from 'vue'

export default {
  props: ['barangays', 'municipality', 'province'],
  emits: ['sortAscending', 'sortDescending'],
  setup() {
    const top = ref(null)
    return {
      top
    }
  },

  methods: {
    // handles scrolling back top of the result
    scrollToTop() {
      this.top.scrollIntoView()
    }
  }
}
</script>

<style scoped>
#to-top {
  position: fixed;
  bottom: 2%;
  right: 1%;
}

.barangay {
  border-radius: 6px;
  background: #457485;
}
</style>
