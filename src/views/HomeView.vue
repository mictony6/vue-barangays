<template>
  <main>
    <TheBanner />
    <MainForm />
  </main>
</template>

<script>
import MainForm from '@/components/MainForm.vue'
import TheBanner from '@/components/TheBanner.vue'
export default {
  components: {
    TheBanner,
    MainForm
  }
}
</script>
