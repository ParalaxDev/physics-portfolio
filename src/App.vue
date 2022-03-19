<template>

    <div class="noise" />
    <div class="navbar" >
        <ul>
            <a class='socials' href='https://github.com/paralaxdev'><span>Github</span></a>
            <a class='socials' href='https://dsc.bio/paralax'><span>Discord</span></a>
        </ul>
        <!-- <a class='theme-switcher' v-on:click="themeSwitch()"> -->
            <!-- <svg class='theme-switcher' v-on:click="themeSwitch()" width="32" height="32" viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M32 64C14.3269 64 0 49.6731 0 32C0 14.3269 14.3269 0 32 0V64Z" :fill="currentPrimary"/>
                <mask id="mask0_2_13" style="mask-type:alpha" maskUnits="userSpaceOnUse" x="32" y="0" width="32" height="64">
                    <rect width="32" height="64" transform="matrix(-1 0 0 1 64 0)" :fill="currentPrimary"/>
                </mask>
                <g mask="url(#mask0_2_13)">
                    <circle r="28" transform="matrix(-1 0 0 1 32 32)" :stroke="currentPrimary" stroke-width="8"/>   
                </g>
            </svg> -->
        <!-- </a> -->
    </div>
    <Header :primary='currentPrimary' :secondary="currentSecondary" />
    
    


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
      themeSwitcher: require('./assets/theme-switcher.svg'),
      PRIMARY: '#40476D',
      SECONDARY: '#FED18C',
      currentPrimary: '#000',
      currentSecondary: '#000',
      width: 1,
    }
  },
  methods: {
      themeSwitch(){
        const root = document.querySelector(':root')
        const rootCompStyled = getComputedStyle(root)
        // console.log(rootCompStyled.getPropertyValue('--primary-color'))

        if (rootCompStyled.getPropertyValue('--primary-color') == this.PRIMARY){
            this.currentPrimary = this.SECONDARY
            this.currentSecondary = this.PRIMARY
        } else {
            this.currentPrimary = this.PRIMARY
            this.currentSecondary = this.SECONDARY
        }

        root.style.setProperty('--primary-color', this.currentPrimary)
        root.style.setProperty('--secondary-color', this.currentSecondary)

      }
  },
  beforeMount() {
    this.prefersDarkScheme = window.matchMedia("(prefers-color-scheme: dark)").matches;
    const root = document.querySelector(':root')
    if (this.prefersDarkScheme){
        this.currentSecondary = this.PRIMARY
        this.currentPrimary = this.SECONDARY
    } else {
        this.currentPrimary = this.PRIMARY
        this.currentSecondary = this.SECONDARY

    }

    root.style.setProperty('--primary-color', this.currentPrimary)
    root.style.setProperty('--secondary-color', this.currentSecondary)

  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@700&display=swap');

/* :root {
      --secondary-color: #D3D0CB;
      --primary-color: #393E41;
}

@media (prefers-color-scheme: dark){
    :root {
      --primary-color: #D3D0CB;
      --secondary-color: #393E41;
    }

} */


* {
    padding: 0;
    margin: 0;
    font-family: 'Roboto Mono', monospace;
    /* background: #D3D0CB; */
    color: var(--primary-color);
}

body {
    overflow: hidden;
    
    background: var(--secondary-color);
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
    padding: 0px 4%;
    flex-grow: 1;
}

.navbar .socials{
    margin-right: 40px;
    font-size: 1.25rem;
    display: inline-block;
    /* float: left; */
    width: fit-content;
    

    text-decoration: none;
    /* text-underline-position: under; */
    transition: 0.25s ;
}
.navbar ul a span::after{
    content: '';
    position: relative;
    width: 100%;
    /* height: 3px; */
    /* background: var(--primary-color); */
    /* top: calc(1.25rem * 2); */
    /* left: 0; */
    border-top: 3px dotted var(--primary-color);
    top: -1.2rem;
    pointer-events: none;
    display: inline-block;
}

.navbar ul a span::after {
    transform-origin: 100% 50%;
    transform: scale3d(0, 1, 1);
    transition: transform 0.3s;
}

.navbar ul a span:hover::after {
    transform-origin: 0% 50%;
    transform: scale3d(1, 1, 1);
}

.navbar ul a {

    width: 100%;

    /* text-decoration-thickness: 3px;  */
}

.navbar ul a span {
    width: inherit;
}

.theme-switcher {
    padding: 0px 75px;
    transition: 0.5s cubic-bezier(.77,0,.175,1);
}

.theme-switcher:hover {
    cursor: pointer;
    transform: rotate(180deg);
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
