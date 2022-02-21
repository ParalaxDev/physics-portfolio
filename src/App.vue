<template>

    <div class="noise" />
    <div class="navbar" >
        <ul>
            <a href='https://github.com/paralaxdev'>Github</a>
            <a href='https://dsc.bio/paralax'>Discord</a>
        </ul>
        <a class='theme-switcher'><img :src='themeSwitcher'  /></a>
    </div>
    <Header />

</template>

<script>
import Header from './components/Header.vue'
// import img from '../assets/theme-switcher.svg'
// import SVGText from './components/SVGText.vue'

export default {
  name: 'App',
  components: {
    Header, 
    // SVGText
  },
  data(){
    return {
      themeSwitcher: require('./assets/theme-switcher.svg')
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@700&display=swap');

* {
    padding: 0;
    margin: 0;
    overflow: hidden;
    font-family: 'Roboto Mono', monospace;
    /* background: #D3D0CB; */
}

body {
    
    background: #D3D0CB;
}

.noise {
    /* background-image: url('/assets/noise.svg'); */
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

.navbar {
    background-color: transparent;
    z-index: 1;
    position: absolute;
    animation: 1s cubic-bezier(.77,0,.175,1) 3s nav forwards;
    /* width: 800px; */
    width: 100%;
    /* padding: 0px 75px; */
    display: flex;

    bottom: -100px;
    /* left: 75px; */
}

.navbar ul{
    padding: 0px 75px;
    flex-grow: 1;
}

.navbar ul a{
    margin-right: 40px;
    font-size: 1.25rem;
    color: #3A3042;
    text-decoration-style: dotted;
    text-decoration-thickness: 2px;
    text-underline-offset: 5px;
    height: 40px;
    display: inline-block;
    /* text-underline-position: under; */
    transition: 0.25s;
}

.navbar ul a:hover {

    text-underline-offset: 7.5px;
    /* text-decoration-thickness: 3px;  */
}

.theme-switcher {
    padding: 0px 75px;
}

.theme-switcher:hover {
    cursor: pointer;
}

.theme-switcher img {
    width: 32px;
    pointer-events: none;
    user-select: none;
}

@keyframes nav {
    0% {
        bottom: -100px;
        opacity: 0;
    } 100% {
        bottom: 75px;
        opacity: 1;
    }
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
