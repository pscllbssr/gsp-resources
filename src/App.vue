<template>
  <div id="app">
    <header>
      <h1>Linksammlung </h1>
      <p>Links, die ich im Rahmen meiner Bachelorarbeit gesammelt habe.</p>
    </header>
    <links></links>
  </div>
</template>

<script>
import Links from './components/Links.vue'
import 'normalize.css'

export default {
  name: 'app',
  components: {
    Links
  }
}
</script>

<style lang="sass">
@import url('https://fonts.googleapis.com/css?family=Montserrat');

html
  color: #424242
  font-family: 'Montserrat', sans-serif
  font-size: 0.8em

header
    display: flex
    min-height: 25vh
    flex-direction: column
    justify-content: center
    align-items: center
    padding: 1em
    background: #4c9ea1
    color: #fff

.btn, .btn-outline
  background: #f5f5f5
  padding: 3px 5px
  cursor: pointer
  outline: 0
  color: #4c9ea1
  font-size: 1rem
  border: none

  &:disabled
    opacity: .5
</style>
