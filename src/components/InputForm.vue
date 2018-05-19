<template>
    <div class="container">
        <form>
            <div class="row">
                <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                    <h1>Media Resource</h1>
                    <hr>
                    <div class="form-group">
                        <label>Media Name</label>
                        <input
                                type="text"
                                id="name"
                                class="form-control" v-model="name">
                    </div>
                    <div class="form-group">
                        <label> Path </label>
                        <input
                                type="text"
                                id="path"
                                class="form-control" v-model="path">
                    </div>
                    <div class="form-group">
                        <label>Media Type</label>
                        <input
                                type="number"
                                id="type"
                                class="form-control" min="0" max="2" v-model="mediaType">
                    </div>
                </div>
            </div>
            <hr>
            <div class="row">
                <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                    <button
                            class="btn btn-primary" @click="post">Post
                    </button>
                    <button class="btn btn-primary" @click.prevent="show"> Show Media
                    </button>
                </div>
            </div>
        </form>
        <hr>
        <table class="table table-striped" v-if="mediaShow">
            <tr>
                <th> Media Name </th>
                <th> Media Path </th>
                <th> Media Type </th>
                <th> Remove</th>
            </tr>
            <tr v-for="media in medias">
                <td>{{media.name}}</td>
                <td><a :href="media.path" v-if="media.path != null"> Go See </a></td>
                <td> {{media.mediaType }}</td>
                <td>
                    <button class="btn btn-primary" @click="removeMedia(media)">Remove</button>
                </td>
            </tr>
        </table>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        data () {
            return {
                name: '',
                path: '',
                mediaType: '',
                medias: [],
                mediaShow: false
            }
        },
        methods: {
            post() {
                const formData = {
                    name: this.name,
                    path: this.path,
                    mediaType: this.mediaType
                }
                axios.post('http://localhost:8100/resource-media/', formData).then(res => {
                    console.log(res)
                }).catch(err => {
                    console.log(err)
                })
            },
            show() {
                this.mediaShow = !this.mediaShow
            },
            removeMedia(media) {
                const url = 'http://localhost:8100/resource-media/' + media.id
                axios.delete(url).then(res => {
                    console.log(res)
                    window.location.reload(true)
                }).catch(err => {
                    console.log(err)
                })
            }
        },
        created() {
            axios.get('http://localhost:8100/resource-media/all').then(res => {
                //console.log('Receive ', res)
                const data = res.data
                const receiveMedia = []
                for (let key in data){
                    const media = data[key]
                    this.medias.push(media)
                    console.log(media)
                }
            }).catch(err => {
                console.log(err)
            })
        }
    }
</script>

<style>
    table, th, td {
        border: 1px solid black;
        text-align: center;
        width:100%;
    }
    div {
        height: auto;
        margin: 0 auto;
    }
</style>