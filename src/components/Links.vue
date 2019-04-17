<template>
  <main>
    <div>
      <h2>Tags: </h2>
      <p>        
          <span v-for="tag in tags" class="tag">
            <label><input type="checkbox" />{{ tag }}</label>
          </span>
      </p>
    </div>
    <box v-for="entry in entries" 
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
      tags: []
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
      return this.entries
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="sass" scoped>

// Small tablets and large smartphones (landscape view)
$screen-sm-min: 576px
@mixin sm 
   @media (min-width: #{$screen-sm-min}) 
       @content;
   


// Small tablets (portrait view)
$screen-md-min: 768px
@mixin md
   @media (min-width: #{$screen-md-min})
       @content;
   


// Tablets and small desktops
$screen-lg-min: 992px
@mixin lg 
   @media (min-width: #{$screen-lg-min}) 
       @content;
   


// Large tablets and desktops
$screen-xl-min: 1200px;
@mixin xl 
   @media (min-width: #{$screen-xl-min}) 
       @content;
   


main 
  display: grid
  grid-gap: 2em
  grid-template-columns: auto
  padding: 3em 1em

  @include sm
    grid-template-columns: 1fr
  
  @include md
    grid-template-columns: 1fr 1fr

  @include lg
    grid-template-columns: 1fr 1fr 1fr
  

</style>
