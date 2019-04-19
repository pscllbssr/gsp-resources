<template>
  <main class="main">
    <div>
      <h2>Filtern nach Stichwörtern:</h2>
      <p>        
          <span v-for="tag in tags" class="tag">
            <label><input type="checkbox" :value="tag" v-model="filter" />{{ tag }}</label>
          </span>
      </p>
      <p>
        <button class="btn btn-outline" @click="filter = []" :disabled="filter.length == 0"> &#x2716; zurücksetzen</button>
      </p>
    </div>
    <box v-for="entry in filtered_entries" 
      :key="entry['gsx$id']['$t']"
      :title="entry['gsx$titel']['$t']"
      :link="entry['gsx$link']['$t']"
      :year="entry['gsx$erscheinungsjahr']['$t']"
      :author="entry['gsx$urheber']['$t']"
      :description="entry['gsx$kommentar']['$t']"
      :tags="entry['gsx$tags']['$t']"
    ></box>
  </main>
</template>

<script>

import axios from 'axios';
import Box from './Box.vue'

export default {
  name: 'Links',
  props: {
    msg: String
  },
  data () {
    return {
      entries: [],
      tags: [],
      filter: []
    }
  },
  mounted () {
    axios
      .get('https://spreadsheets.google.com/feeds/list/1Woq-IZwwE__qwRupX2ptw_gfjG1C0w2pg8GUNwDIo84/1/public/values?alt=json')
      .then(response => (this.entries = response.data.feed.entry))
      .then( () => {
        this.entries.forEach((entry) => {
            let tags_text = entry['gsx$tags']['$t']
            this.tags = [...new Set(this.tags.concat(tags_text.split(',')))]
        });
      })
  },
  components: {
    Box
  },
  computed: {
    filtered_entries: function () {
      if( this.filter.length == 0 ){
        return this.entries
      }else{
        return this.entries.filter(j => {
            let found = false
            this.filter.every( (filter_str) => {
              if (j['gsx$tags']['$t'].includes(filter_str)){
                found = true
                return false
              }else{
                return true
              }
            });
            return found
          })
      } 
    }
  }
}
</script>

<style lang="sass">   

// Small tablets and large smartphones (landscape view)
$screen-sm-min: 650px
@mixin sm 
   @media (min-width: #{$screen-sm-min}) 
       @content;

// Small tablets (portrait view)
$screen-md-min: 850px
=md
   @media (min-width: #{$screen-md-min})
       @content;

// Tablets and small desktops
$screen-lg-min: 1100px
=lg 
   @media (min-width: #{$screen-lg-min}) 
       @content
   
// Large tablets and desktops
$screen-xl-min: 1350px
=xl 
   @media (min-width: #{$screen-xl-min}) 
       @content;
       
.main
  display: grid
  grid-gap: 2em
  grid-template-columns: 1fr
  padding: 3em 1em

  +sm
    grid-template-columns: 1fr 1fr
  
  +md
    grid-template-columns: 1fr 1fr 1fr

  +lg
    grid-template-columns: 1fr 1fr 1fr 1fr
  
span.tag
    background: #f5f5f5
    margin: 0 5px 5px 0
    padding: 0.5em
    font-size: 0.8rem
    cursor: pointer
    white-space: nowrap
    display: inline-block

    input
      margin-right: 3px
      vertical-align: middle
    
    label
      cursor: pointer
</style>
