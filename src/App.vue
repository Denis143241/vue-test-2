<template>
  <component :is="layout">
    <router-view/>
  </component>
</template>

<script>
import EmptyLayout from '@/layouts/EmptyLayout.vue'
import MainLayout from '@/layouts/MainLayout.vue'
import DurakLayout from '@/layouts/DurakLayout.vue'

console.log()

export default {
  computed: {
    layout() {
      return (this.$route.meta.layout || 'durak') + '-layout'
    }
  },
  components: {
    EmptyLayout, MainLayout, DurakLayout
  }
}
</script>

<style lang="less">
  @import '~materialize-css/dist/css/materialize.min.css';
  @import 'assets/style.less';
</style>
