<template>
    <svg width="100px" height="100px" viewBox="0 0 2000 2000" xmlns="http://www.w3.org/2000/svg" >
            <path d='M624 934L369 1456H41L450 734L30 0H362L628 530L895 0H1223L803 734L1213 1456H883L624 934Z' />
    </svg>


    <!-- <div ref="physicsArea" id="physics-header"></div> -->
</template>

<script>

import Matter from 'matter-js'

// this.render = require('../render')

export default {
    name: 'Header',
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
                Mouse = Matter.Mouse

            this.engine = Engine.create();

            this.render = Render.create({
                element: this.$refs.physicsArea,
                engine: this.engine,
                options: {
                    width: window.innerWidth,
                    height: window.innerHeight,
                    background: 'rgba(0,0,0,0)',
                    wireframes: false,
                }
            });
            this.title = document.querySelector('.title')   
            this.paths = this.title.querySelectorAll('path')

            var ground = Bodies.rectangle(window.innerWidth/2, window.innerHeight, window.innerWidth, 60, { isStatic: true });
            
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
            this.addLetters()

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
            
        },

        addLetters () {
            this.paths.forEach(path => {
            // const letter = path.attributes.class.value.split('-')[1]
            console.log(this.convertToURI(`<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 2096 74" width="30" height="30" /><path d="${path.attributes.d.value}/></svg>`))
            // this.convertToURI('<svg viewBox="0 0 2096 74"><path d="M 2073.193 12.207 L 2095.117 12.207 L 2095.117 1.026 L 2037.793 1.026 L 2037.793 12.207 L 2059.424 12.207 L 2059.424 72.119 L 2073.193 72.119 L 2073.193 12.207 Z" /></svg>')

            // data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 2096 74'%3E%3Cpath d='M 2073.193 12.207 L 2095.117 12.207 L 2095.117 1.026 L 2037.793 1.026 L 2037.793 12.207 L 2059.424 12.207 L 2059.424 72.119 L 2073.193 72.119 L 2073.193 12.207 Z' /%3E%3C/svg%3E
            const bounding = path.getBoundingClientRect()
            // const texture = PIXI.


            const rectangle = Matter.Bodies.rectangle(
                bounding.left + bounding.width / 2, 
                bounding.top + bounding.height / 2, 
                bounding.width,
                bounding.height,
                {
                    // isSleeping: true,
                    collisionFilter: {
                        category: 0x0004
                    },
                    render: {
                        sprite: {
                            // texture: [add uri here]
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
        convertToURI: function (raw) {

            var encoded = raw.replace(/\s+/g, " ")

            encoded = this.replaceAll(encoded, "%", "%25"); 
            encoded = this.replaceAll(encoded, "> <", "><"); // normalise spaces elements
            encoded = this.replaceAll(encoded, "; }", ";}"); // normalise spaces css
            encoded = this.replaceAll(encoded, "<", "%3c");
            encoded = this.replaceAll(encoded, ">", "%3e");
            encoded = this.replaceAll(encoded, "\"", "'");
            encoded = this.replaceAll(encoded, "#", "%23"); // needed for ie and firefox
            encoded = this.replaceAll(encoded, "{", "%7b");
            encoded = this.replaceAll(encoded, "}", "%7d");     
            encoded = this.replaceAll(encoded, "|", "%7c");
            encoded = this.replaceAll(encoded, "^", "%5e");
            encoded = this.replaceAll(encoded, "`", "%60"); 
            encoded = this.replaceAll(encoded, "@", "%40"); 

            // charset reportedly not needed ... I need to test before implementing
            var uri = 'data:image/svg+xml;charset=UTF-8,' + encoded;
            return uri
        },


        escapeRegExp: function(string) {
            return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'); // $& means the whole matched string
        },
        replaceAll: function(str, match, replacement){
            return str.replace(new RegExp(this.escapeRegExp(match), 'g'), ()=>replacement);
        },
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
    /* overflow: clip; */
}

.title {
  display: block;
  position: absolute;
  top: 50%;
  left: 50px;
  width: 873px;
  height: auto;
  
  opacity: 0;
  pointer-events: none;
  transform: translate3d(0, -50%, 0);
  z-index: 2;
}

</style>
