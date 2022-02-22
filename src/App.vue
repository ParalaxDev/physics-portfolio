<template>

    <div class="noise" />
    <div class="navbar" >
        <ul>
            <a class='socials' href='https://github.com/paralaxdev'>Github</a>
            <a class='socials' href='https://dsc.bio/paralax'>Discord</a>
        </ul>
        <a class='theme-switcher' v-on:click="themeSwitch()">
            <svg width="32" height="32" viewBox="0 0 64 64" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M32 64C14.3269 64 0 49.6731 0 32C0 14.3269 14.3269 0 32 0V64Z" :fill="PRIMARY"/>
                <mask id="mask0_2_13" style="mask-type:alpha" maskUnits="userSpaceOnUse" x="32" y="0" width="32" height="64">
                    <rect width="32" height="64" transform="matrix(-1 0 0 1 64 0)" :fill="PRIMARY"/>
                </mask>
                <g mask="url(#mask0_2_13)">
                    <circle r="28" transform="matrix(-1 0 0 1 32 32)" :stroke="PRIMARY" stroke-width="8"/>   
                </g>
            </svg>
        </a>
    </div>
    <Header :primary='PRIMARY' :secondary="SECONDARY" scale=1 />
    
    


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
      PRIMARY: '#393E41',
      SECONDARY: '#D3D0CB'
    }
  },
  methods: {
      themeSwitch(){
        const root = document.querySelector(':root')
        const rootCompStyled = getComputedStyle(root)
        // console.log(rootCompStyled.getPropertyValue('--primary-color'))

        if (rootCompStyled.getPropertyValue('--primary-color') == '#393E41'){
            this.PRIMARY = '#D3D0CB'
            this.SECONDARY = '#393E41'
        } else {
            this.PRIMARY = '#393E41'
            this.SECONDARY = '#D3D0CB'
        }

        root.style.setProperty('--primary-color', this.PRIMARY)
        root.style.setProperty('--secondary-color', this.SECONDARY)

      }
  },
  beforeMount() {
    this.prefersDarkScheme = window.matchMedia("(prefers-color-scheme: dark)").matches;
    if (this.prefersDarkScheme){
        this.SECONDARY = '#393E41'
        this.PRIMARY = '#D3D0CB'
    } else {
        this.PRIMARY = '#393E41'
        this.SECONDARY = '#D3D0CB'

    }

  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@700&display=swap');

:root {
      --secondary-color: #D3D0CB;
      --primary-color: #393E41;
}

@media (prefers-color-scheme: dark){
    :root {
      --primary-color: #D3D0CB;
      --secondary-color: #393E41;
    }

}


* {
    padding: 0;
    margin: 0;
    overflow: hidden;
    font-family: 'Roboto Mono', monospace;
    /* background: #D3D0CB; */
    color: var(--primary-color);
}

body {
    
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
    padding: 0px 75px;
    flex-grow: 1;
}

.navbar .socials{
    margin-right: 40px;
    font-size: 1.25rem;

    text-decoration-style: dotted;
    text-decoration-thickness: 2.5px;
    text-underline-offset: 40px;
    height: 50px;
    display: inline-block;
    /* text-underline-position: under; */
    transition: 0.25s ;
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

.theme-switcher svg {
    transition: 0.5s cubic-bezier(.77,0,.175,1);
}

.theme-switcher svg:hover {
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
