<template>
  <RouterView />
</template>

<script>
import './assets/main.css'
import 'bulma/css/bulma.min.css'
import { RouterView } from 'vue-router'

export default {
  components: {
    RouterView
  }
}
</script>

<style scoped></style>
