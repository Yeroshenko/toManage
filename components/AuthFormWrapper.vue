<template>
  <div class="auth-page">
    <ShadowBlock>
      <h1 class="auth-page__title">To Manage</h1>
      <slot />
    </ShadowBlock>
  </div>
</template>

<script>
import ShadowBlock from '@/components/ShadowBlock'
import '@/styles/pages/auth.sass'

export default {
  components: { ShadowBlock }
}
</script>

<style></style>
