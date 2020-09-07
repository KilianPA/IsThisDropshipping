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
      <div v-if="pictures.length" class="q-col-md-12 q-col-xs-12 q-pa-md row">
          <q-img         style="width: 100px"
                         :ratio="1" :src="picture" v-for="picture in pictures" :key="picture"/>
      </div>
    </div>
  </div>
</template>

<script>
import ItemTextLink from './Helpers/ItemTextLink'
const cheerio = require('cheerio')
export default {
  name: 'AliExpressItem',
  components: { ItemTextLink },
  props: ['website'],
  inject: ['baseUrl'],
  computed: {},
  data () {
    return {
      pictures: []
    }
  },
  mounted () {
    this.getInfo()
  },
  methods: {
    getInfo () {
      this.$axios.get(this.baseUrl + this.website).then(response => {
        const scrap = cheerio.load(response.data)
        scrap('img').filter((i, el) => {
          if (el.parent.name === 'a') {
            if (el.attribs.src) {
              if (this.pictures.length < 4) {
                this.pictures.push(el.attribs.src)
              }
            }
          }
        })
      }).catch(error => {
        if (error) {
          console.log('error')
        }
      })
    }
  },
  watch: {}
}
</script>
<style lang="stylus">
</style>
