<!-- @vue-ignore -->
<script>
export default {
  name: 'SectionWrapper',
  props: {
    padded: {
      type: Boolean,
      default: true,
    },
  },
}
</script>

<template>
  <section :class="{'lg:p-8': padded}">
    <slot />
  </section>
</template>
