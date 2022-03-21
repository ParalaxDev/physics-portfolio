<template>

    <!-- <div id='loader'> -->
        
    <!-- </div> -->
    <div class="noise" />
    <svg class="spinner" v-on:click='spawnObject' width="32px" height="32px" viewBox="0 0 116 104" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:space="preserve" xmlns:serif="http://www.serif.com/" style="fill-rule:evenodd;clip-rule:evenodd;stroke-linejoin:round;stroke-miterlimit:2;" role="button" aria-roledescription="spawn in a physics object">
        <g transform="matrix(1,0,0,1,-198.102,-204.305)">
            <g transform="matrix(2.69229e-17,-0.439684,0.439684,2.69229e-17,143.441,368.559)">
            <path d="M229.664,150.655C229.664,136.11 241.455,124.319 256,124.319C270.545,124.319 282.336,136.11 282.336,150.655L282.336,210.384L334.063,180.52C346.66,173.247 362.767,177.563 370.039,190.159C377.312,202.756 372.996,218.863 360.399,226.135L308.672,256L360.399,285.865C372.996,293.137 377.312,309.244 370.039,321.841C362.767,334.437 346.66,338.753 334.063,331.48L282.336,301.616L282.336,361.345C282.336,375.89 270.545,387.681 256,387.681C241.455,387.681 229.664,375.89 229.664,361.345L229.664,301.616L177.937,331.48C165.34,338.753 149.233,334.437 141.961,321.841C134.688,309.244 139.004,293.137 151.601,285.865L203.328,256L151.601,226.135C139.004,218.863 134.688,202.756 141.961,190.159C149.233,177.563 165.34,173.247 177.937,180.52L229.664,210.384L229.664,150.655Z" :fill='currentPrimary' />
            </g>
        </g>
    </svg>

    <div class="navbar" >
        <ul>
            <a class='socials' href='https://github.com/paralaxdev'><span>Github</span></a>
            <a class='socials' href='https://dsc.bio/paralax'><span>Discord</span></a>
        </ul>
    </div>

    <Header :primary='currentPrimary' :secondary="currentSecondary" ref='header' aria-roledescription="physics based header"/>
    
    


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
      PRIMARY: '#000D59 ',
      SECONDARY: '#f4b41a',
      currentPrimary: '#000',
      currentSecondary: '#000',
      width: 1,
    }
  },
  methods: {
      spawnObject() {
        this.$refs.header.spawnObject()
      },
      themeSwitch(){
        const root = document.querySelector(':root')
        const rootCompStyled = getComputedStyle(root)
        // const favicon = document.getElementById("favicon");
        // console.log(rootCompStyled.getPropertyValue('--primary-color'))

        if (rootCompStyled.getPropertyValue('--primary-color') == this.PRIMARY){
            this.currentPrimary = this.SECONDARY
            this.currentSecondary = this.PRIMARY
            // favicon.href = '@/favicon-dark.ico'
        } else {
            this.currentPrimary = this.PRIMARY
            this.currentSecondary = this.SECONDARY
            // favicon.href = '@/favicon-light.ico'
        }

        root.style.setProperty('--primary-color', this.currentPrimary)
        root.style.setProperty('--secondary-color', this.currentSecondary)

      }
  },
  beforeMount() {
    this.prefersDarkScheme = window.matchMedia("(prefers-color-scheme: dark)").matches;
    const root = document.querySelector(':root')
    // const favicon = document.getElementById("favicon");

    if (this.prefersDarkScheme){
        this.currentSecondary = this.PRIMARY
        this.currentPrimary = this.SECONDARY
        // favicon.href = '@/favicon-dark.ico'
    } else {
        this.currentPrimary = this.PRIMARY
        this.currentSecondary = this.SECONDARY
        // favicon.href = '@/favicon-light.ico'

    }

    
    root.style.setProperty('--primary-color', this.currentPrimary)
    root.style.setProperty('--secondary-color', this.currentSecondary)

    // setInterval(() => {
    
    // }, 100)
    // console.log(document.readyState)

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
      -webkit-touch-callout: none; /* iOS Safari */
    -webkit-user-select: none; /* Safari */
     -khtml-user-select: none; /* Konqueror HTML */
       -moz-user-select: none; /* Old versions of Firefox */
        -ms-user-select: none; /* Internet Explorer/Edge */
            user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}

body {
    overflow: hidden;
    
    
    background: var(--secondary-color);
}

#loader {
    background: var(--secondary-color);
    width: 100vw;
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 100;
    position: relative;
    animation: finished 1.8s 1.8s forwards;
}

.spinner {
    /* animation: spinner 1.8s infinite; */
    padding: 0px 75px;
    top: -100px;
    animation: 1s cubic-bezier(.77,0,.175,1) 3s spin forwards;
    position: relative;

    transition: 1s;
    z-index: 100;

}

@media (max-width:600px) {
    .spinner {
        padding: 0px 50px !important;
        
    }

    .navbar ul {
        padding: 0px 50px !important;
        /* flex-grow: 1; */
    }
}

.spinner:hover {
    /* animation: spinner 1.8s infinite; */
    transform: rotate(180deg);
    cursor: pointer;
    /* padding: 0px 75px;
    top: -100px;
    animation: 1s cubic-bezier(.77,0,.175,1) 3s spin forwards;
    position: relative; */

    /* transition: 1s; */

}

@keyframes spin {
    0% {
        top: -100px;
        opacity: 0;
    } 100% {
        top: 10vh;
        opacity: 1;
    }
}

@keyframes spinner {
    0% {
        transform: rotate(0);
        animation-timing-function: cubic-bezier(0.55, 0.055, 0.675, 0.19);
    }
    50% {
        transform: rotate(180deg);
        animation-timing-function: cubic-bezier(0.215, 0.61, 0.355, 1);
    }
    100% {
        transform: rotate(360deg);
    }
}

.noise {
    background-image: url('/assets/noise-light.svg');
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
    background-size:30vh 30vh;
    will-change:transform
}
@media (prefers-color-scheme: dark) {
    .noise {
        background-image: url('/assets/noise-dark.svg') !important;
    }
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
    display: inline-block;
    /* float: left; */
    width: fit-content;
    

    text-decoration: none;
    /* text-underline-position: under; */
    transition: 0.25s cubic-bezier(.77,0,.175,1) ;
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
