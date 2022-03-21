<template>

    <SVGText text='Im a~Front end~DEveloper' :scale="this.scale"/>
    <div ref="physicsArea" id="physics-header"></div>
</template>

<script>
import SVGText from './SVGText.vue'
import Matter from 'matter-js'

// this.render = require('../render')

export default {
    name: 'Header',
    components: {
        SVGText
    },
    data() {
        return {
            groundID: 1,
            isMobile: /iPhone|iPad|iPod|Android/i.test(navigator.userAgent) || window.innerWidth < 600,
            scale: /iPhone|iPad|iPod|Android/i.test(navigator.userAgent) || window.innerWidth < 600 ? window.innerWidth * 0.0029 : window.innerWidth * 0.001953125
        }
    },
    props: {
        primary: String,
        secondary: String,
        width: Number,
    },
    watch: { 
        primary: function() {

            console.log('changed')

            for (let i = 0; i < 5; i++) {
                
                this.engine.world.bodies.forEach(body => {
                    if (!body.isStatic){
    
                        Matter.Composite.remove(this.engine.world, body)
                    }
                });
                
            }
            this.addLetters(this.scale)
        }
    },
    methods: {
        randInt(min, max) { // min and max included 
            return Math.floor(Math.random() * (max - min + 1) + min)
        },
        spawnObject() {
            const rectangle = Matter.Bodies.rectangle(
                this.randInt(0, window.innerWidth * 0.7), 0, 35, 35,
                {
                    // isSleeping: true,
                    // restitution: 0.1,
                    frictionAir: this.getRandomNum(0, 0.006),
                    // frictionAir: 0.02,
                    // friction: 50,
                    // frictionStatic: 100,
                    density: 0.06,
                    render: {
                        sprite: {
                            texture: "data:image/svg+xml,%3Csvg width='35' height='35' viewBox='0 0 116 104' version='1.1' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' xml:space='preserve' xmlns:serif='http://www.serif.com/' style='fill-rule:evenodd;clip-rule:evenodd;stroke-linejoin:round;stroke-miterlimit:2;'%3E%3Cg transform='matrix(1,0,0,1,-198.102,-204.305)'%3E%3Cg transform='matrix(2.69229e-17,-0.439684,0.439684,2.69229e-17,143.441,368.559)'%3E%3Cpath d='M229.664,150.655C229.664,136.11 241.455,124.319 256,124.319C270.545,124.319 282.336,136.11 282.336,150.655L282.336,210.384L334.063,180.52C346.66,173.247 362.767,177.563 370.039,190.159C377.312,202.756 372.996,218.863 360.399,226.135L308.672,256L360.399,285.865C372.996,293.137 377.312,309.244 370.039,321.841C362.767,334.437 346.66,338.753 334.063,331.48L282.336,301.616L282.336,361.345C282.336,375.89 270.545,387.681 256,387.681C241.455,387.681 229.664,375.89 229.664,361.345L229.664,301.616L177.937,331.48C165.34,338.753 149.233,334.437 141.961,321.841C134.688,309.244 139.004,293.137 151.601,285.865L203.328,256L151.601,226.135C139.004,218.863 134.688,202.756 141.961,190.159C149.233,177.563 165.34,173.247 177.937,180.52L229.664,210.384L229.664,150.655Z' fill='%23"+this.primary.split('#')[1]+"' /%3E%3C/g%3E%3C/g%3E%3C/svg%3E%0A"
                        }   
                    },
                    collisionFilter: {
                        category: 0x0008,
                        mask: 0x0001 | 0x0004
                    },
                }
            )

            Matter.Composite.add(this.engine.world, [rectangle]);

        },
        init: function() {

            


            // module aliases
            var Engine = Matter.Engine,
                Render = Matter.Render,
                Runner = Matter.Runner,
                Bodies = Matter.Bodies,
                Composite = Matter.Composite,
                // Composites = Matter.Composites,
                // Common = Matter.Common,
                MouseConstraint = Matter.MouseConstraint,
                Mouse = Matter.Mouse,
                Events = Matter.Events
                // World = Matter.World
                // Detector = Matter.Detector

            this.engine = Engine.create();

            this.render = Render.create({
                element: this.$refs.physicsArea,
                engine: this.engine,
                options: {
                    width: window.innerWidth,
                    height: window.innerHeight,
                    background: 'transparent',
                    wireframes: false,
                    // showCollisions: true,
                    // showBounds: true,
                    showSleeping: false,
                }
            });
            this.title = document.querySelector('.title')   
            setTimeout(() => {
                
                this.paths = this.title.querySelectorAll('path')
                this.addLetters(this.scale)
                // console.log(this.paths)
            }, 1000)

            // const bounding = this.title.getBoundingClientRect()

            // console.log(bounding.width)

            let x = this.isMobile ? (window.innerWidth) / 2 : (window.innerWidth * 0.65) / 2  + (window.innerWidth * 0.03)
            let y = this.isMobile ? window.innerHeight * 0.65 : window.innerHeight * 0.75
            let w = this.isMobile ? window.innerHeight : window.innerWidth * 0.65

            var ground = Bodies.rectangle(x, y, w, 20, { isStatic: true, render: { visible: false }, collisionFilter: {category: 0x0002} });
            this.groundID = ground.id
            // console.log(ground.id)

            // var collider = Bodies.rectangle(window.innerWidth*10 / 2, window.innerHeight + 400, window.innerWidth*10, 100, {isStatic: true, isSensor: true} )
          
            
            
            var mouse = Mouse.create(this.render.canvas)
            var mouseConstraint = MouseConstraint.create(this.engine, {
                mouse: mouse,
                constraint: {
                    stiffness: 0.2,
                    render: {
                        visible: false
                    }
                }
            });

            // console.log(this.engine.world)


            Composite.add(this.engine.world, [ground]);
            // add all of the bodies to the world

            Composite.add(this.engine.world, mouseConstraint);

            // run the renderer
            Render.run(this.render);

            // create runner
            var runner = Runner.create();

            // run the engine
            Runner.run(runner, this.engine);

            Events.on(runner, "beforeUpdate", () => {
                this.engine.world.bodies.forEach(body => {
                    if(body.position.y > window.innerHeight + 200){
                        Composite.remove(this.engine.world, body)
                        // console.log('removed')
                        // console.log(this.engine.world.bodies.length)
                        if (this.engine.world.bodies.length <= 1){
                            this.addLetters(this.scale)
                            // console.log('added letters')
                        }
                    }
                });
            })
            
        },

        addLetters (scale) {
            this.paths.forEach(path => {
            // const letter = path.attributes.class.value.split('-')[1]
            // console.log(path)
            // console.log(path.attributes)
            // const svgURI = this.convertToURI(`<svg width='${100 * this.scale}px' height='${100 * this.scale}px' viewBox='0 0 ${4000} ${1000}' xmlns='http://www.w3.org/2000/svg'  transform='translate(${155 - (17.5 * (this.scale - 1))}, ${60 + (22 * (this.scale - 1))})' ><path d='${path.attributes.d.value}' transform='scale (1, -1)' style=' fill: ${this.primary}'></path></svg>`)
            const svgURI = this.convertToURI(`<svg width='${100 * scale}px' height='${100 * scale}px' viewBox='0 0 ${4000} ${1000}' xmlns='http://www.w3.org/2000/svg'  transform='translate(${35 * scale}px, ${30 * scale}px)'  style="-webkit-transform: translate(${35 * scale}px, ${30 * scale}px);" ><path d='${path.attributes.d.value}' transform='scale (1, -1)' style=' fill: ${this.primary}'></path></svg>`)
            // this.convertToURI('<svg viewBox="0 0 2096 74"><path d="M 2073.193 12.207 L 2095.117 12.207 L 2095.117 1.026 L 2037.793 1.026 L 2037.793 12.207 L 2059.424 12.207 L 2059.424 72.119 L 2073.193 72.119 L 2073.193 12.207 Z" /></svg>')

            // data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 2096 74'%3E%3Cpath d='M 2073.193 12.207 L 2095.117 12.207 L 2095.117 1.026 L 2037.793 1.026 L 2037.793 12.207 L 2059.424 12.207 L 2059.424 72.119 L 2073.193 72.119 L 2073.193 12.207 Z' /%3E%3C/svg%3E
            const bounding = path.getBoundingClientRect()
            // console.log(bounding)
            // const texture = PIXI.

            // console.log(bounding.width)


            const rectangle = Matter.Bodies.rectangle(
                bounding.left + bounding.width / 2, 
                bounding.top + bounding.height / 2, 
                bounding.width,
                bounding.height,
                {
                    // isSleeping: true,
                    // restitution: 0.1,
                    // frictionAir: this.getRandomNum(0, 0.006),
                    // frictionAir: 0.02,
                    // friction: 50,
                    // frictionStatic: 100,
                    // density: 10000,
                    collisionFilter: {
                        category: 0x0004
                    },
                    render: {
                        sprite: {
                            texture: svgURI
                        }   
                    }
                }
            )
            
            Matter.Composite.add(this.engine.world, rectangle)

        })
            
        },

    
        windowResizeHandler: function () {

            this.isMobile = /iPhone|iPad|iPod|Android/i.test(navigator.userAgent) || window.innerWidth < 600;



            this.render.canvas.width =  window.innerWidth
            this.render.canvas.height = window.innerHeight

            for (let i = 0; i < 5; i++) {
                
                this.engine.world.bodies.forEach(body => {
                    // if (!body.isStatic){
                        
                        Matter.Composite.remove(this.engine.world, body)
                    // }
                });
                
            }

            setTimeout(() => {
                console.log(this.isMobile)
                
                this.scale = this.isMobile ? window.innerWidth * 0.0029 : window.innerWidth * 0.001953125
                let x = this.isMobile ? (window.innerWidth) / 2 : (window.innerWidth * 0.65) / 2  + (window.innerWidth * 0.03)
                let y = this.isMobile ? window.innerHeight * 0.65 : window.innerHeight * 0.75
                let w = this.isMobile ? window.innerHeight : window.innerWidth * 0.65
                // let w = 


                var ground = Matter.Bodies.rectangle(x, y, w, 20, { isStatic: true, render: { visible: false }, collisionFilter: {category: 0x0002} });
                this.groundID = ground.id

                Matter.Composite.add(this.engine.world, [ground]);
    
                this.addLetters(this.scale)
            }, 1)

            // this.$forceUpdate()

        },
        convertToURI: function (data) {

            const symbols = /[\r\n%#()<>?[\\\]^`{|}]/g;
            data = data.replace(/'/g, `"`);


            data = data.replace(/>\s{1,}</g, `><`);
            data = data.replace(/\s{2,}/g, ` `);

            // Using encodeURIComponent() as replacement function
            // allows to keep result code readable
            return 'data:image/svg+xml,' + data.replace(symbols, encodeURIComponent);
            
        },
        getRandomNum: function(min, max) {
            return Math.random() * (max - min) + min;
        }

    },






    mounted: function() {
        

        this.init()
    },

    created() {
        // this.$root.$refs.Header = this
        window.addEventListener("resize", this.windowResizeHandler);
    },
    unmounted() {
        window.removeEventListener("resize", this.windowResizeHandler);
    },

}
</script>

<style scoped>

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

#physics-header {
    position: absolute;
    /* box-sizing: border-box; */
    outline: 30px solid var(--primary-color);
    /* outline-offset: -15px; */
    animation: 0.5s ease-out 3.5s borderAnimate forwards;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

.title {
  display: block;
  position: absolute;
  /* top: 25%; */
  /* left: 50px; */
  /* width: 1000px; */
  height: auto;
  
  opacity: 0;
  pointer-events: none;
  transform: translate3d(-5%, -100%, 0);
  z-index: 2;
}

@media (max-width:600px) {
    #physics-header{

       animation: 0.5s ease-out 3.5s borderAnimateMobile forwards !important;
    }
}



@keyframes borderAnimate {
    0% {
        outline-offset: 0px;
    } 100% {
        outline-offset: -15px;
    }
}
@keyframes borderAnimateMobile {
    0% {
        outline-offset: 0px;
    } 100% {
        outline-offset: -10px;
    }
}

</style>
