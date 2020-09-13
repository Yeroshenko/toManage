<template>
  <div class="shadow-block">
    <slot />
  </div>
</template>

<script>
export default {}
</script>

<style lang="sass">
.shadow-block
  background-color: var(--color-white)
  padding: 24px 16px

  min-width: 400px
  border-radius: var(--vs-radius)
  transition: all .25s ease
  box-shadow: 0 5px 30px 0 rgba(var(--color-black), .05)
  max-width: 800px
</style>
