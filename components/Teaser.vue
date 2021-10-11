<template>
  <div>
    ---- Teaser: {{ data ? data.title_bold : '' }} ----
  </div>
</template>
<script>
  export default {
    data() {
      return {}
    },
    props: {
      data: {}
    }
  }

</script>
