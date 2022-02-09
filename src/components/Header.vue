<template>
    <div ref="physicsArea" id="physics-header"></div>
</template>

<script>

import Matter from 'matter-js'

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
                Composites = Matter.Composites,
                Common = Matter.Common,
                MouseConstraint = Matter.MouseConstraint,
                Mouse = Matter.Mouse

            // create an engine
            var engine = Engine.create();

            // create a renderer
            this.render = Render.create({
                element: this.$refs.physicsArea,
                engine: engine,
                options: {
                    width: window.innerWidth,
                    height: window.innerHeight,
                    background: 'rgba(0,0,0,0)',
                    wireframes: false,
                }
            });

            // create two boxes and a ground
            var boxA = Bodies.rectangle(400, 200, 80, 80);
            var boxB = Bodies.rectangle(450, 50, 80, 80);
            var stack = Composites.stack(window.innerWidth/4, 0, 10, 10, 1, 15, function(x, y) {
                return Bodies.circle(x, y, Common.random(15, 30), { restitution: 0.6, friction: 0.1 });
            }); 
            var ground = Bodies.rectangle(window.innerWidth/2, window.innerHeight, window.innerWidth, 60, { isStatic: true });
            

            var mouse = Mouse.create(this.render.canvas)
            var mouseConstraint = MouseConstraint.create(engine, {
                mouse: mouse,
                constraint: {
                    stiffness: 0.2,
                    render: {
                        visible: false
                    }
                }
            });

            // add all of the bodies to the world
            Composite.add(engine.world, [boxA, boxB, ground, stack]);

            Composite.add(engine.world, mouseConstraint);

            // run the renderer
            Render.run(this.render);

            // create runner
            var runner = Runner.create();

            // run the engine
            Runner.run(runner, engine);
            
        },
    },
    mounted: function() {

        this.init()
    }

}
</script>

<style scoped>

#physics-header {
    overflow: clip;
}

</style>
