<template>
  <div id="app" v-cloak>
    <nav class="navbar navbar-light bg-faded">
      <h1 class="navbar-brand mb-0">{{title}}</h1>
    </nav>
    <Search/>
  </div>
</template>

<script>
import Search from './components/Search'

export default {
  name: 'app',
  components: {
    Search
  },
  data () {
    return {
      title: 'Mini Search Engine'
    }
  }
}
</script>

<style>
[v-cloak] {
	display: none;
}
</style>
