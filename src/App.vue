<template>
  <FreestyleDispenser />
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import FreestyleDispenser from './components/FreestyleDispenser.vue'

export default defineComponent({
  name: 'App',
  components: {
    FreestyleDispenser
  }
})
</script>
