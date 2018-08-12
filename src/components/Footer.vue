<template functional>
  <footer>
    <p><span>{{ footerText }}</span></p>
  </footer>
</template>

<script>
export default {
  name: 'Footer',
  computed: {
    footerText: function () {
      return '&copy; ' + new Date().getFullYear()
    }
  }
}
</script>
