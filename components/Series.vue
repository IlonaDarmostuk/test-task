<template>
  <div>
    {{data.current}} - {{data.type}} - {{data.id}} - {{data.slug}} - {{data.name}} - {{data.series ? data.series.length : ''}}
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
