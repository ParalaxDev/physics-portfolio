<template>
    <h1>{{ text }}</h1>
    <svg width="100px" height="100px" viewBox="0 0 4000 2000" xmlns="http://www.w3.org/2000/svg" class="svg-area">

    </svg>
</template>
<script>

// import font from '../assets/roboto-mono-700.svg'

export default {
    props: {
        text: String
    },
    methods: {
        init() {

            // var doc = null

            fetch('assets/roboto-mono-700.svg')
                .then(response => response.text())
                .then((text) => {
                    const nodes = parser.parseFromString(text, "image/svg+xml").documentElement.childNodes
                    const svg = document.getElementsByTagName('svg')[0]

                    const letters = this.text.split('')

                    letters.forEach((letter, i) => {
                        nodes.forEach(node => {
                            if(node.attributes){
    
                                const nodeLetter = node.attributes.item(0).nodeValue
                                const nodePath = node.attributes.item(1).nodeValue

                                if (nodeLetter == letter.toUpperCase()){

                                    console.log(`${nodeLetter}: ${nodePath}`)
                                    // const svg = document.createElement(svg)
                                    var newElement = document.createElementNS("http://www.w3.org/2000/svg", 'path')
                                    newElement.setAttribute("d", nodePath); //Set path's data
                                    newElement.setAttribute("style", `x: ${100 * i}px`)
                                    var transformAttr = ' translateX(' + 100 * i + ')';
                                    newElement.setAttribute('transform', transformAttr);
                                    // newElement.style.stroke = "#000"; //Set stroke colour
                                    // newElement.style.strokeWidth = "5px"; //Set stroke width
                                    svg.appendChild(newElement);
                                }
    
                            }
                        });
                        
                    });


                    // console.log(letters)

                    
                })
            const parser = new DOMParser()
            
            // console.log(font)
            // console.log(doc)
        }
    },
    mounted() {
        this.init()
    }
    
}
</script>
<style scoped>

.svg-area{
    position: absolute;
}

</style>