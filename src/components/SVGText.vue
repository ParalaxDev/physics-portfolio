<template>
    <h1>{{ text }}</h1>
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
                    nodes.forEach(node => {
                        if(node.attributes){

                            const letter = node.attributes.item(0).nodeValue
                            const path = node.attributes.item(1).nodeValue


                            console.log(`${letter}: ${path}`)
                        }
                    });
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

</style>