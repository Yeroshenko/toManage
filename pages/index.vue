<template>
  <div class="page">
    <CanvasBg />
    <vs-button size="large">Click me</vs-button>
  </div>
</template>

<script>
import CanvasBg from '@/components/CanvasBg'

export default {
  components: { CanvasBg },
}
</script>
<style>
.page {
  position: relative;
  height: 100vh;
  cursor: auto;
  display: flex;
  align-items: center;
  justify-content: center;
}

.page__inner {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
