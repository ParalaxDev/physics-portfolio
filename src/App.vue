<template>

    <div class="noise" />
    <Header />

</template>

<script>
import Header from './components/Header.vue'
// import SVGText from './components/SVGText.vue'

export default {
  name: 'App',
  components: {
    Header, 
    // SVGText
  },
}
</script>

<style>

* {
    padding: 0;
    margin: 0;
    overflow: hidden;
}

.noise {
    background-image: url('/assets/noise.svg');
    background-repeat: repeat;  
    display:block;
    position:fixed;
    top:-100px;
    right:-100px;
    bottom:-100px;
    left:-100px;
    content:'';
    z-index:-1;
    -webkit-animation:noise .6s steps(1) infinite;
    animation:noise .6s steps(1) infinite;
    background-size:250px 250px;
    will-change:transform
}

@keyframes noise {
 0% {
  transform:translate3d(0,0,0)
 }
 10% {
  transform:translate3d(-100px,-100px,0)
 }
 20% {
  transform:translate3d(100px,-90px,0)
 }
 30% {
  transform:translate3d(-90px,80px,0)
 }
 40% {
  transform:translate3d(80px,-70px,0)
 }
 50% {
  transform:translate3d(-70px,60px,0)
 }
 60% {
  transform:translate3d(60px,-50px,0)
 }
 70% {
  transform:translate3d(-50px,40px,0)
 }
 80% {
  transform:translate3d(40px,-30px,0)
 }
 90% {
  transform:translate3d(-30px,20px,0)
 }
 100% {
  transform:translate3d(20px,-10px,0)
 }
}

</style>
