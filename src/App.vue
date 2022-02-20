<template>
  <SVGText text='Hello World' size='2' />
</template>

<script>
// import Header from './components/Header.vue'
import SVGText from './components/SVGText.vue'

export default {
  name: 'App',
  components: {
    // Header, 
    SVGText
  }
}
</script>

<style>

* {
    padding: 0;
    margin: 0;
    /* overflow: hidden; */
}

</style>
