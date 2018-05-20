<template>
    <main-layout>
        <div class="row" style="padding: 10px 20px">
            <h1>Ajouter une composition </h1>
            <div class="row" style="padding-bottom: 30px">
                <button class="btn btn-primary" @click="addCompoAndRedirect()">Valider</button>
                <v-link class="btn btn-secondary" href="/">Annuler</v-link>
            </div>
        </div>

        <div class="row" style="min-height: 500px">
            <div class="col-md-6">
                <h4>Aperçu :</h4>
                <p v-if="!compo.id">Votre composition n'est pas encore créée</p>
                <p v-if="compo.id">{{compo}}</p>
                <div v-if="compo.module && compo.module.type === '.SplitView'" id="paneSplit" class='components-container' style="min-height: 400px;">
                    <split-pane v-on:resize="resize" :min-percent='20' v-bind:split="getSplitType(compo.module)">

                        <!--
                        <template slot="paneL">
                            A
                        </template>

                        <template slot="paneR">

                            <split-pane split="horizontal">

                                <template slot="paneL">
                                    B
                                </template>

                                <template slot="paneR">
                                    <split-pane v-on:resize="resize" split="vertical" :default-percent='30'>

                                        <template slot="paneL">
                                            C
                                        </template>

                                        <template slot="paneR">
                                            D
                                        </template>

                                    </split-pane>
                                </template>

                            </split-pane>

                        </template>
                        -->
                    </split-pane>
                </div>
            </div>
            <div class="col-md-6">
                <div style="min-height: 350px; max-height: 350px; overflow: auto">
                    <div class="row">
                        <div class="col-md-12" style="padding-bottom: 30px">
                            <p>Diviser l'écran de manière :</p>
                            <p><button class="btn btn-default col-md-6" @click="addHorizSplit">Horizontale</button></p>
                            <p><button class="btn btn-default col-md-6" @click="addVerticSplit">Verticale</button></p>
                        </div>
                        <div class="col-md-12">
                            SLIDE
                        </div>
                    </div>

                </div>
                <div style="min-height: 350px; max-height: 350px; overflow: auto">
                    <h4>Médias disponibles :</h4>
                    <table class="table table-striped">
                        <tr>
                            <th> Nom </th>
                            <th> Aperçu </th>
                            <th> Type </th>
                        </tr>
                        <tr v-for="media in medias">
                            <td>{{media.name}}</td>
                            <td><a :href="media.path" v-if="media.path != null" target="_blank">Voir le média</a>
                            </td>
                            <td> {{media.mediaType }}</td>
                        </tr>
                    </table>
                </div>
            </div>
        </div>
    </main-layout>
</template>

<script>
    import MainLayout from '../layouts/Main.vue';
    import VLink from '../components/VLink.vue';
    import splitPane from '../components/index.vue'

    export default {
        components: {
            MainLayout,
            VLink,
            splitPane
        },
        data() {
            return {
                compo: {id: ''},
                medias: this.getMedias()
            }
        },
        methods: {
            getHorizontalSplit() {

            },
            getVerticalSplit() {

            },
            getSplitType(mod) {
                return mod.typeSplit.toLowerCase();
            },
            getMedias() {
                this.$http.get('http://localhost:8100/resource-media/all')
                    .then(response => {
                        return response.json();
                    }).then(data => {
                    const resultArray = [];
                    for (let key in data) {
                        resultArray.push(data[key]);
                    }
                    this.medias = resultArray;
                }).catch(err => {
                    console.log(err)
                })
            },
            resize() {
                console.log('resize')
            },
            addCompoAndRedirect() {
                this.$root.currentRoute = '/';
            },
            addCompo() {
                this.$http.post('http://localhost:8200/compositions', this.compo)
                    .then((result) =>{
                        return result.json();
                    })
                    .then((compoCreated) => this.compo = compoCreated);
            },
            updateCompo() {
                this.$http.put('http://localhost:8200/compositions', this.compo)
                    .then((result) =>{
                        return result.json();
                    })
                    .then((compoUpdated) => this.compo = compoUpdated);
            },
            addHorizSplit() {
                let horizSpli = {
                    "type": ".SplitView",
                    'typeSplit': 'HORIZONTAL'
                };

                if (this.compo.id) {
                    // UPDATE
                    console.log('update');
                    console.log(this.compo['module'].id);
                    if (!this.compo['module'].id || (this.compo['module'].id && this.compo['module'].type !== '.SplitView')) {
                        console.log('%%%% IF');

                        this.$http.post('http://localhost:8200/split-views', horizSpli)
                            .then((mod) => {
                                const module =  mod.body;
                                module['content1'] = this.compo['module'].id ? this.compo['module'] : null;
                                this.compo['module'] = module;
                                this.updateCompo();
                            });
                    } else if (this.compo['module'].type === '.SplitView') {
                        console.log('%%%% ELSE');
                        this.$http.post('http://localhost:8200/split-views', horizSpli)
                            .then((response) => {
                                const module = response.body;
                                this.compo['module'].content1 = module;
                            });
                    }
                } else {
                    // CREATE
                    this.compo['module'] = horizSpli;
                    this.addCompo();
                }
            },
            addVerticSplit() {
                console.log('addVerticSplit');

                let verticalSplit = {
                    "type": ".SplitView",
                    'typeSplit': 'VERTICAL'
                };

                if (this.compo.id) {
                    // UPDATE
                    console.log('update');
                    console.log(this.compo['module'].id);
                    if (!this.compo['module'].id || (this.compo['module'].id && this.compo['module'].type !== '.SplitView')) {
                        console.log('%%%% IF');
                        verticalSplit['content1'] = this.compo['module'].id ? this.compo['module'] : null;
                        this.compo['module'] = verticalSplit;
                    } else if (this.compo['module'].type === '.SplitView') {
                        console.log('%%%% ELSE');
                        this.compo['module'].content1 = verticalSplit;
                    }
                    this.updateCompo();
                } else {
                    // CREATE
                    this.compo['module'] = verticalSplit;
                    this.addCompo();
                }
            }
        }
    }
</script>

<style>
    .components-container {
        position: relative;
        height: 500px;
    }
</style>
