<template lang="pug">
  #app
    pm-header
    section.section
      nav.nav.has-shadow
        .container
          input.input.is-large(type="text", placeholder="buscar canciones", v-model="searchQuery")
          a.button.is-info.is-large(@click="search") Buscar
          a.button.is.danger.is-large &times;

      .container
        p
          small {{ searchMessage }}
      
      .container.results
        .columns
          .column(v-for = "t in tracks")| {{t.name}} - {{t.artists[0].name}}

    pm-footer
</template>

<script>
import trackService from './services/track'
import PmFooter from './components/layout/Footer.vue'
import PmHeader from './components/layout/Header.vue'

export default {
  name: 'app',

  components: {PmFooter, PmHeader},

  data () {
    return {
      searchQuery:'',
      tracks: []
    }
  },

  computed: {
    searchMessage(){
      return 'encontradas '+ this.tracks.length;
    }
  },

  methods: {
    search(){
      if(this.searchQuery === '') {return}
      //console.log(this.searchQuery)
      trackService.search(this.searchQuery)
        .then(res => {
          this.tracks = res.tracks.items
          //console.log(res)
        })
    }
  }
}
</script>

<style lang="scss">
@import './scss/main.scss'

</style>
