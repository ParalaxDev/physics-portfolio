<template>

    <SVGText text='Frontend~DEveloper' size=3 />

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
                    // showBounds: true,
                    showSleeping: false,
                }
            });
            this.title = document.querySelector('.title')   
            setTimeout(() => {
                
                this.paths = this.title.querySelectorAll('path')
                this.addLetters()
                console.log(this.paths)
            }, 1000)

            // const bounding = this.title.getBoundingClientRect()


            var ground = Bodies.rectangle(550, window.innerHeight - 260, 1000, 5, { isStatic: true, render: { visible: false } });

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
                        console.log(this.engine.world.bodies.length)
                        if (this.engine.world.bodies.length <= 1){
                            this.addLetters()
                            console.log('added letters')
                        }
                    }
                });
            })
            
        },

        addLetters () {
            this.paths.forEach(path => {
            // const letter = path.attributes.class.value.split('-')[1]
            console.log(path)
            const svgURI = this.convertToURI(`<svg width='350px' height='350px' viewBox='0 0 4000 1000' xmlns='http://www.w3.org/2000/svg'  transform='translate(120, 20)' ><path d='${path.attributes.d.value}' transform='scale (1, -1)' style='transform-origin: center; fill: #B0413E'></path></svg>`)
            // this.convertToURI('<svg viewBox="0 0 2096 74"><path d="M 2073.193 12.207 L 2095.117 12.207 L 2095.117 1.026 L 2037.793 1.026 L 2037.793 12.207 L 2059.424 12.207 L 2059.424 72.119 L 2073.193 72.119 L 2073.193 12.207 Z" /></svg>')

            // data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 2096 74'%3E%3Cpath d='M 2073.193 12.207 L 2095.117 12.207 L 2095.117 1.026 L 2037.793 1.026 L 2037.793 12.207 L 2059.424 12.207 L 2059.424 72.119 L 2073.193 72.119 L 2073.193 12.207 Z' /%3E%3C/svg%3E
            const bounding = path.getBoundingClientRect()
            // console.log(bounding)
            // const texture = PIXI.


            const rectangle = Matter.Bodies.rectangle(
                bounding.left + bounding.width / 2, 
                bounding.top + bounding.height / 2, 
                bounding.width,
                bounding.height,
                {
                    // isSleeping: true,
                    // restitution: this.getRandomNum(0, 0.25),
                    frictionAir: this.getRandomNum(0, 0.006),
                    // frictionAir: 0.006,
                    friction: 100,
                    collisionFilter: {
                        category: 0x0004
                    },
                    render: {
                        // fillStyle: '#000000',
                        // strokeStyle: 'blue',
                        // lineWidth: 3,
                        sprite: {
                            // texture: "data:image/svg+xml,%3Csvg width='1000px' height='100px' viewBox='0 0 13000 2000' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M1142 456H861Q857 390 839 343T790 266Q759 236 715 222T616 208Q575 208 542 217T481 247Q456 265 437 292T403 354Q383 405 374 473T364 629V827Q364 891 370 946T389 1048Q406 1107 434 1151T504 1220Q528 1234 556 1241T618 1248Q680 1248 724 1232T799 1183Q828 1150 843 1101T863 987H1143Q1133 1101 1095 1192T991 1346Q925 1409 832 1443T618 1477Q537 1477 467 1457T337 1398Q271 1353 219 1288T135 1140Q107 1072 93 993T79 825V629Q79 535 94 451T139 297Q169 227 212 169T312 70Q373 26 449 3T616 -21Q730 -21 824 12T986 108Q1054 170 1094 258T1142 456Z' style='transform-origin: center;' transform='translate(1100, 0), scale (1, -1)'%3E%3C/path%3E%3C/svg%3E"
                            // texture: 'data:image/svg+xml,%3Csvg width="1000px" height="100px" viewBox="0 0 13000 2000" xmlns="http://www.w3.org/2000/svg"%3E%3Cpath d="M1142 456H861Q857 390 839 343T790 266Q759 236 715 222T616 208Q575 208 542 217T481 247Q456 265 437 292T403 354Q383 405 374 473T364 629V827Q364 891 370 946T389 1048Q406 1107 434 1151T504 1220Q528 1234 556 1241T618 1248Q680 1248 724 1232T799 1183Q828 1150 843 1101T863 987H1143Q1133 1101 1095 1192T991 1346Q925 1409 832 1443T618 1477Q537 1477 467 1457T337 1398Q271 1353 219 1288T135 1140Q107 1072 93 993T79 825V629Q79 535 94 451T139 297Q169 227 212 169T312 70Q373 26 449 3T616 -21Q730 -21 824 12T986 108Q1054 170 1094 258T1142 456Z" style="transform-origin: center;" transform="translate(1100, 0), scale (1, -1)"%3E%3C/path%3E%3C/svg%3E'
                            // texture: 'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 2096 74" width="30" height="30" /%3E%3Cpath d="M1201 1227V1456H27V1227H470V0H752V1227H1201Z/%3E%3C/svg%3E'
                            // texture: "data:image/svg+xml,%3csvg width='1000px' height='100px' viewBox='0 0 13000 2000' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M1142 456H861Q857 390 839 343T790 266Q759 236 715 222T616 208Q575 208 542 217T481 247Q456 265 437 292T403 354Q383 405 374 473T364 629V827Q364 891 370 946T389 1048Q406 1107 434 1151T504 1220Q528 1234 556 1241T618 1248Q680 1248 724 1232T799 1183Q828 1150 843 1101T863 987H1143Q1133 1101 1095 1192T991 1346Q925 1409 832 1443T618 1477Q537 1477 467 1457T337 1398Q271 1353 219 1288T135 1140Q107 1072 93 993T79 825V629Q79 535 94 451T139 297Q169 227 212 169T312 70Q373 26 449 3T616 -21Q730 -21 824 12T986 108Q1054 170 1094 258T1142 456Z' style='transform-origin: center;' transform='translate(1100, 0), scale (1, -1)'%3e%3c/path%3e%3c/svg%3e"
                            texture: svgURI
                        }   
                    }
                }
            )
            
            Matter.Composite.add(this.engine.world, rectangle)

        })
            
        },

    
        windowResizeHandler: function () {
            this.render.options.width =  window.innerWidth
            this.render.options.height = window.innerHeight
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

#physics-header {
    position: absolute;
    /* box-sizing: border-box; */
    outline: 15px solid #B0413E;
    /* outline-offset: -15px; */
    animation: 1s cubic-bezier(.77,0,.175,1) 1.5s borderAnimate forwards;
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
