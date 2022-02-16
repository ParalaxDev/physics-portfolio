<template>
    <h1>{{ text }}</h1>
</template>
<script>

import font from '../assets/roboto-mono-700.svg'

export default {
    props: {
        text: String
    },
    methods: {
        init() {

            const parser = new DOMParser()
            const doc = parser.parseFromString(font, "image/svg+xml")

            console.log(doc)
        }
    },
    mounted() {
        this.init()
    }
    
}
</script>
<style scoped>

</style>