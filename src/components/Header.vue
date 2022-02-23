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
            scale: window.innerWidth * 0.001953125
        }
    },
    props: {
        primary: String,
        secondary: String,
        width: Number,
    },
    watch: { 
        primary: function() {
            for (let i = 0; i < 5; i++) {
                
                this.engine.world.bodies.forEach(body => {
                    if (!body.isStatic){
    
                        Matter.Composite.remove(this.engine.world, body)
                    }
                });
                
            }
            this.addLetters(window.innerWidth * 0.001953125)
        }
    },
    methods: {
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
                    showBounds: true,
                    showSleeping: false,
                }
            });
            this.title = document.querySelector('.title')   
            setTimeout(() => {
                
                this.paths = this.title.querySelectorAll('path')
                this.addLetters(window.innerWidth * 0.001953125)
                // console.log(this.paths)
            }, 1000)

            // const bounding = this.title.getBoundingClientRect()

            // console.log(bounding.width)


            var ground = Bodies.rectangle((window.innerWidth * 0.65) / 2  + (window.innerWidth * 0.032), window.innerHeight * 0.75, window.innerWidth * 0.65, 20, { isStatic: true, render: { visible: false } });
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
                            this.addLetters(window.innerWidth * 0.001953125)
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
            const svgURI = this.convertToURI(`<svg width='${100 * scale}px' height='${100 * scale}px' viewBox='0 0 ${4000} ${1000}' xmlns='http://www.w3.org/2000/svg'  transform='translate(${35 * scale}, ${30 * scale})' ><path d='${path.attributes.d.value}' transform='scale (1, -1)' style=' fill: ${this.primary}'></path></svg>`)
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
                    friction: 100,
                    frictionStatic: 100,
                    // density: 10000,
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

                var ground = Matter.Bodies.rectangle((window.innerWidth * 0.65) / 2  + (window.innerWidth * 0.032), window.innerHeight - 180 , window.innerWidth * 0.65, 5, { isStatic: true, render: { visible: false } });
                Matter.Composite.add(this.engine.world, [ground]);
    
                this.addLetters(window.innerWidth * 0.001953125)
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
    animation: 0.5s ease-out 3s borderAnimate forwards;
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

@keyframes borderAnimate {
    0% {
        outline-offset: 0px;
    } 100% {
        outline-offset: -15px;
    }
}

</style>
