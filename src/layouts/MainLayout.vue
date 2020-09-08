<template xmlns:v-slot="http://www.w3.org/1999/XSL/Transform">
  <div class="content-center">
    <div class="container-input q-mt-lg">
      <div class="full-width text-center q-mt-lg q-mb-lg">
        <span class="text-h4"> Is this dropshipping ? </span>
      </div>
      <q-input :loading="loading" outlined v-model="url" placeholder="URL" >
        <template v-slot:prepend>
          <q-avatar v-if="url" size="50px" square>
            <q-img :src="url"/>
          </q-avatar>
        </template>
        <template v-slot:append>
          <q-icon v-if="url !== ''" name="close" @click="url = ''" class="cursor-pointer" />
          <q-icon @click.native="imageSearch" class="cursor-pointer" name="search" />
        </template>
      </q-input>
      <q-card v-if="everSearch" class="q-mt-md">
        <div class="row">
          <div class="col-md-12 col-xs-12 border-top"  v-for="(website, index) in websiteFound" :key="index">
            <ali-express-item v-if="website.includes('aliexpress')" :website="website"/>
            <basic-item v-else :website="website"/>
          </div>
          <div v-if="!websiteFound.length">
            <span class="text-italic text-grey">
              No item found
            </span>
          </div>
        </div>
      </q-card>
    </div>
  </div>
</template>

<script>
import BasicItem from './Item/BasicItem'
import AliExpressItem from './Item/AliExpressItem'
export default {
  name: 'MainLayout',
  components: { AliExpressItem, BasicItem },
  data () {
    return {
      websiteFound: [],
      everSearch: false,
      loading: false,
      url: 'https://cdn.shopify.com/s/files/1/0413/6878/0954/products/H954ef7ad059f4e70a62a7859f139b13fV_1024x1024@2x.jpg?v=1596582752',
      dataHtml: '',
      filteredWebsite: ['aliexpress', 'zalando', 'amazon'],
      removedWebsite: ['google', 'w3', 'schema', 'gstatic', 'twitter'],
      allWebsite: [],
      baseUrl: 'http://api.scrapestack.com/scrape?access_key=8b578b3d3f896fbecb1c35fe670f4f36&url='
      // baseUrl: 'https://api.scraperapi.com?api_key=203bc1297a170cddc24c75b42f44adca&url='
    }
  },
  mounted () {
  },
  methods: {
    goTo (website) {
      window.open(website, '_blank')
    },
    imageSearch () {
      this.loading = true
      this.allWebsite = []
      this.websiteFound = []
      this.$axios.get(this.baseUrl + 'https://www.google.com/searchbyimage?image_url=' + encodeURIComponent(this.url) + '&btnG=Recherche+par+image&encoded_image=&image_content=&filename=&hl=fr')
        .then(response => {
          console.log(response)
          this.everSearch = true
          this.searchInHtml(response.data)
        }).catch(error => {
          if (error) {
            Object.keys(error).forEach(key => {
              console.log(error[key])
            })
            this.everSearch = true
            this.loading = false
          }
        })
    },
    searchInHtml (str) {
      const regEx = /\b(https?:\/\/.*?\.[a-z]{2,4}\/[^\s]*\b)/g
      this.allWebsite = (str.match(regEx))
      this.allWebsite = this.cleanWebsite(this.allWebsite)
      this.allWebsite.forEach(site => {
        this.filteredWebsite.forEach(filter => {
          if (site.includes(filter)) {
            if (/\d/.test(site)) {
              this.websiteFound.push(site)
            }
          }
        })
      })
      this.loading = false
    },
    cleanWebsite (website) {
      const arr = []
      website.forEach(site => {
        let flag = false
        this.removedWebsite.forEach(removeWebsite => {
          if (site.includes(removeWebsite)) {
            flag = true
          }
        })
        if (!flag) {
          arr.push(site)
        }
      })
      return arr
    }
  },
  provide () {
    return {
      baseUrl: this.baseUrl
    }
  }
}
</script>

<style>
  .container-input {
    width: 70%;
    margin-left: auto;
    margin-right: auto;
  }
  .border-top {
    border-top: solid 1px #efefef
  }
</style>
