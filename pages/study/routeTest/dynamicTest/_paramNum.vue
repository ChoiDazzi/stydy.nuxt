<template>
  <div>
    <p>parameter: {{ parameter }}</p>
  </div>
</template>

<script>
export default {
  layout: 'main-layout',
  data() {
    return {
      parameter: this.$route.params.paramNum,
    }
  },

}
</script>
