<template>
    <section :id='this.id' class='project-item'>
        <img class='thumbnail' :src="'/assets/thumbnails/' + this.img + '.png'" />
        <div class='container'>
            <h1>{{this.title.toUpperCase()}}</h1>
            <h3>{{this.description}}</h3>
            <a href="https://graphics.paralax.xyz/projects/kew">➡️</a>
        </div>
    </section>
</template>

<script>
export default {
    props : {
        title: String,
        img: String,
        description: String,
        id: String
    }
}
</script>

<style scoped>
.project-item {
    /* background: rgba(255, 0, 0, 0.333); */
    height: 100vh;
    width: 100vw;
    position: fixed;
    /* top: 100vh; */
    transform-origin: 100vw;
    transform: rotate(160deg);
    display: flex;
    justify-content: right;
    align-items: center;

}

.container {
    height: calc(50% + 30px);
    padding: 0;
}

h1 {
    font-size: 2rem;
}

h3 {
    font-size: 1rem;
    
}


.thumbnail {
    height: 30%;
    outline: 7.5px solid var(--primary-color);
    margin-right: 50px;
}
</style>