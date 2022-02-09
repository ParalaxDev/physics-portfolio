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
            var ground = Bodies.rectangle(400, 610, 810, 60, { isStatic: true });

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
            Composite.add(engine.world, [boxA, boxB, ground]);

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
