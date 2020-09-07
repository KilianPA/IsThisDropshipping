<template>
    <a class="text-href text-secondary" target="_blank" :href="website"> {{ website }} </a>
</template>

<script>
export default {
  name: 'ItemTextLink',
  props: ['website'],
  inject: [],
  computed: {},
  data () {
    return {}
  },
  mounted () {},
  methods: {},
  watch: {}
}
</script>
<style lang="stylus">
</style>
