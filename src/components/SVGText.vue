<template>
    <!-- <h1>{{ text }}</h1> -->
    <svg width="1000px" height="100px" viewBox="0 0 13000 2000" xmlns="http://www.w3.org/2000/svg" class="title">

    </svg>
</template>
<script>

// import font from '../assets/roboto-mono-700.svg'

export default {
    props: {
        text: String,
        size: Number,
    },
    methods: {
        init() {

            // var doc = null

            fetch('assets/roboto-mono-700.svg')
                .then(response => response.text())
                .then((text) => {
                    const nodes = parser.parseFromString(text, "image/svg+xml").documentElement.childNodes
                    const svg = document.getElementsByTagName('svg')[0]
                    var y = 0
                    var x = 0

                    const letters = this.text.split('')
                    console.log(letters.length)


                    letters.forEach((letter) => {
                        x += 1
                        if (letter == '~'){
                            y += 1
                            x = 0
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
                                    newElement.setAttribute("style", `transform-origin: center;`)
                                    // var transformAttr = ' translateX("' + 100 * i + 'px")';
                                    // var transformAttr = 'translateX("100px")';
                                    newElement.setAttribute('transform', 'translate(' + 1100*x + ', ' + 2200 * y + '), scale (1, -1)');
                                    // newElement.style.stroke = "#000"; //Set stroke colour
                                    // newElement.style.strokeWidth = "5px"; //Set stroke width
                                    svg.appendChild(newElement);
                                }
    
                            }
                        });
                        
                    });

                    svg.setAttribute('viewBox', `0 0 ${x * 1200} ${2200 * (y)}`)
                    svg.setAttribute('width', `${x * 40 * this.size}px`)
                    svg.setAttribute('height', `${250 * (y ) * this.size}px`)
                    console.log(y)

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