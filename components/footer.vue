<template>
  <a-layout-footer :style="{ textAlign: 'center' }">
      Ant Design ©{{ currentYear }} Created by Ant UED
  </a-layout-footer>
</template>

<script lang="ts">
  const currentYear: number = new Date().getFullYear()

  export default {
    data() {
      return {
        currentYear,
      }
    }
  }
</script>