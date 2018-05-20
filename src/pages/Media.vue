<template>
    <main-layout>
    <div id="app">
        <div class="row" style="padding-bottom: 10px">
            <button class="btn btn-primary" @click="showAddMedia = !this.showAddMedia" style="float: left; margin-left: 15px">Ajouter Média</button>
        </div>
        <app-media v-if="showAddMedia" @close="showAddMedia = false "></app-media>
        <table class="table table-striped">
            <tr>
                <th></th>
                <th> Nom du Média </th>
                <th> URL </th>
                <th> Type du Média </th>
            </tr>
            <tr v-for="media in medias">
                <td style="width: 50px;">
                    <button class="btn btn-danger" @click.prevent="removeMedia(media)">Supprimer</button>
                </td>
                <td>{{media.name}}</td>
                <td><a href="#" v-if="media.path != null"> {{media.path}} </a></td>
                <td> {{media.mediaType }}</td>
            </tr>
        </table>
    </div>

    </main-layout>
</template>

<script>
    import Media from  '../components/InputForm.vue'
    import MainLayout from '../layouts/Main.vue';
    import axios from 'axios'
    import { compoBus } from '../main.js'
    export default {
        components: {
            'app-media': Media,
            'main-layout': MainLayout
        },
        data() {
            return {
                'showAddMedia': false,
                medias: [],
            }
        },
        methods: {
            removeMedia(media) {
                const url = 'http://localhost:8100/resource-media/' + media.id
                axios.delete(url).then(res => {
                    console.log(res)
                    this.medias = []
                    axios.get('http://localhost:8100/resource-media/all').then(res => {
                        const data = res.data
                        for (let key in data){
                            const media = data[key]
                            this.medias.push(media)
                            console.log(media)
                        }
                    })
                }).catch(err => {
                    console.log(err)
                })
            }
        },
        created() {
            axios.get('http://localhost:8100/resource-media/all').then(res => {
                //console.log('Receive ', res)
                const data = res.data
                for (let key in data){
                    const media = data[key]
                    this.medias.push(media)
                    console.log(media)
                }
            }).catch(err => {
                console.log(err)
            })
            compoBus.$on('postNewMedia', (media) => {
                this.medias.push(media)
            });
        },
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
    table {
        color: #333;
        font-family: Helvetica, Arial, sans-serif;
        width: 640px;
        border-collapse:
                collapse; border-spacing: 0;
        text-align: center;
    }

    td, th {
        border: 1px solid transparent; /* No more visible border */
        height: 30px;
        text-align: center;
        transition: all 0.3s;  /* Simple transition for hover effect */
    }

    th {
        background: #DFDFDF;  /* Darken header a bit */
        font-weight: bold;
    }

    td {
        background: #FAFAFA;
        text-align: center;
    }

    /* Cells in even rows (2,4,6...) are one color */
    tr:nth-child(even) td { background: #F1F1F1; }

    /* Cells in odd rows (1,3,5...) are another (excludes header cells)  */
    tr:nth-child(odd) td { background: #FEFEFE; }

    tr:hover { background: #666; color: rgba(98, 11, 16, 0.65); }
    /* Hover cell effect! */

    div {
        height: auto;
        margin: 0 auto;
    }
</style>