<template>
    <main-layout>
    <div id="app">
        <app-media></app-media>
    </div>
    </main-layout>
</template>

<script>
    import Media from  '../components/InputForm.vue'
    import MainLayout from '../layouts/Main.vue';
    export default {
        components: {
            'app-media': Media,
            'main-layout': MainLayout
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
    h1, h2 {
        font-weight: normal;
    }
    ul {
        list-style-type: none;
        padding: 0;
    }
    li {
        display: inline-block;
        margin: 0 10px;
    }
    a {
        color: #42b983;
    }
</style>