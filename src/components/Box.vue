<template>
  <a :href="link" target="_blank" :title="link">
      <h2>{{ title }}</h2>
      <p class="meta"><i>{{ year }} &bull; {{ author }}</i></p>
      <p class="content">{{ description }}</p>
      <p>
          <span v-for="tag in tag_arr" class="tag">{{ tag }}</span>
      </p>
  </a>
</template>

<script>

export default {
  name: 'Box',
  props: {
    link: String,
    title: String,
    year: String,
    author: String,
    description: String,
    tags: String
  },computed: {
    tag_arr: function () {
      return this.tags.split(',')
    }
  }
}
</script>
<style lang="sass">
a
    display: flex
    flex-direction: column
    box-shadow: #E0E0E0 1px 1px 5px
    padding: 1em
    color: #424242
    text-decoration: none
    border-top: 3px solid #4c9ea1
    transition: box-shadow 200ms
    &:hover
      box-shadow: #E0E0E0 2px 2px 7px  

p.meta
    font-size: 0.8rem
    margin: 0

p.content
    flex: 1
    line-height: 1.5
</style>

