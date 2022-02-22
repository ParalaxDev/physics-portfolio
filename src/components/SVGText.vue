<template>
    <!-- <h1>{{ text }}</h1> -->
    <svg width="1000px" height="100px" viewBox="0 0 13000 2000" xmlns="http://www.w3.org/2000/svg" class="title">

    </svg>
</template>
<script>

// import font from '../assets/roboto-mono-700.svg'

export default {
    data() {
        return {
            scale: window.innerWidth * 0.001953125
        }
    },
    props: {
        text: String,
        size: Number,
        xOffset: Number,
        yOffset: Number,
        // scale: Number
    },
    methods: {
        init() {
            console.log('ran')

            // var doc = null

            fetch('assets/roboto-mono-700.svg')
                .then(response => response.text())
                .then((text) => {
                    const nodes = parser.parseFromString(text, "image/svg+xml").documentElement.childNodes
                    // const svg = document.getElementsByTagName('svg')[0]
                    const svg = document.querySelector('.title')
                    this.y = 0
                    this.x = 0

                    const letters = this.text.split('')
                    // console.log(letters.length)


                    letters.forEach((letter) => {
                        this.x += 1
                        if (letter == '~'){
                            this.y += 1
                            this.x = 0
                        }
                        nodes.forEach(node => {
                            if(node.attributes){
                                
                                const nodeLetter = node.attributes.item(0).nodeValue
                                const nodePath = node.attributes.item(1).nodeValue

                                if (nodeLetter == letter.toUpperCase()){

                                    // console.log(`${nodeLetter}: ${nodePath}`)
                                    // const svg = document.createElement(svg)
                                    var newElement = document.createElementNS("http://www.w3.org/2000/svg", 'path')
                                    newElement.setAttribute("d", nodePath); //Set path's data
                                    newElement.setAttribute("style", `transform-origin: center; fill: red`)
                                    // newElement.setAttribute("fill", "red")
                                    // var transformAttr = ' translateX("' + 100 * i + 'px")';
                                    // var transformAttr = 'translateX("100px")';
                                    newElement.setAttribute('transform', `translate(${1200*this.x}, ${2200*this.y}), scale(1, -1)`);
                                    // newElement.setAttribute('transform', `translate(${this.xOffset*this.x}, ${this.yOffset * this.y}), scale(1, -1)`);
                                    // newElement.setAttribute('transform', `translate(${this.xOffset*this.x}, ${this.yOffset * this.y}), scale(${this.scale}), scale(1, -1)`);
                                    // newElement.style.stroke = "#000"; //Set stroke colour
                                    // newElement.style.strokeWidth = "5px"; //Set stroke width
                                    svg.appendChild(newElement);
                                }
    
                            }
                        });
                        
                    });

                    svg.setAttribute('viewBox', `0 0 ${this.x * 1400} ${2300 * (this.y)}`)
                    svg.setAttribute('width', `${this.x * 40 * this.scale}px`)
                    svg.setAttribute('height', `${250 * (this.y) * this.scale}px`)
                    // console.log(this.y)

                    // console.log(letters)

                    
                })
            const parser = new DOMParser()
            
            // console.log(font)
            // console.log(doc)
        },
        windowResize() {

            const svg = document.querySelector('.title')

            // const letters = this.text.split('')


            // svg.setAttribute('viewBox', `0 0 ${this.x * 1400} ${2300 * this.y}`)
            svg.setAttribute('width', `${this.x * 40 * window.innerWidth * 0.001953125}px`)
            svg.setAttribute('height', `${250 * (this.y) * window.innerWidth * 0.001953125}px`)

            console.log(`${this.x * 40 * window.innerWidth * 0.001953125}px`)
        }
    },
    mounted() {
        this.init()
    },
    created() {
        window.addEventListener("resize", this.windowResize);
    },
    unmounted() {
        window.removeEventListener("resize", this.windowResize);
    },

    
}
</script>
<style scoped>

.svg-area{
    position: absolute;
}

</style>