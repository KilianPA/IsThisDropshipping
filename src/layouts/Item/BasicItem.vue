<template>
  <div class="col-md-12 col-xs-12 q-pa-md q-mt-sm">
    <div class="row">
      <div class="col-md-10 col-xs-10">
        <q-item-label class="text-no-wrap ellipsis">
          <item-text-link :website="website"/>
        </q-item-label>
      </div>
      <div class="col-md-2 col-xs-2 text-right">
        <q-icon class="cursor-pointer" @click.native="goTo(website)" name="launch"/>
      </div>
    </div>
  </div>
</template>

<script>
import ItemTextLink from './Helpers/ItemTextLink'
export default {
  name: 'BasicItem',
  components: { ItemTextLink },
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
