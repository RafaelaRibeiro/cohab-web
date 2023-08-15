<template>
  <div class="settings-container">
    <t-header />
    <t-menu-setting />
    <t-content />
  </div>
</template>

<script>
import TContent from '../components/templates/TContent.vue'
import THeader from '../components/templates/THeader.vue'
import TMenuSetting from '../components/templates/TMenuSetting.vue'

export default {
  components: { THeader, TContent, TMenuSetting },
  name: 'setting',
  data() {
    return {
      drawer: true,
    }
  },
}
</script>

<style></style>
