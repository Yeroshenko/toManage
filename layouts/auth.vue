<template>
  <div class="auth-layout">
    <CanvasBg />
    <Nuxt />
  </div>
</template>

<script>
import CanvasBg from '@/components/CanvasBg'
import '@/styles/layouts/auth.sass'

export default {
  components: { CanvasBg }
}
</script>
