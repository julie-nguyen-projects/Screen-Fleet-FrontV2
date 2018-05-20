<template>
    <main-layout>
        <div class="col-md-6">
            <h1>Compositions</h1>
            <div class="row" style="padding-bottom: 10px">
                <v-link class="btn btn-primary" href="/compo">Ajouter une composition</v-link>
            </div>
            <div class="row">
                <ul class="list-group" style="max-height: 500px; overflow: auto">
                    <li class="list-group-item" v-for="c in this.compositions">
                        <button class="btn btn-primary" @click.prevent="editCompo(c)">Modifier</button>
                        <button class="btn btn-danger" @click.prevent="deleteCompo(c)">Supprimer</button>
                        {{c.id}}
                    </li>
                </ul>
            </div>
        </div>

        <div class="col-md-6">
            <h1>Télévisions</h1>
            <div class="row" style="padding-bottom: 10px">
                <button class="btn btn-primary" @click="showAddTv = true">Ajouter une TV</button>
            </div>
            <div class="row">
                <ul class="list-group" style="max-height: 500px; overflow: auto">
                    <li class="list-group-item" v-for="tv in this.tvs">
                        <button class="btn btn-primary" @click.prevent="modifyTv(tv)">Modifier</button>
                        <button class="btn btn-danger" @click.prevent="delTv(tv)">Supprimer</button>
                        {{ tv.name }} | {{ tv.ipAdress }} | {{ tv.compositionId }}
                    </li>
                </ul>
            </div>
            <button class="btn btn-primary" @click.prevent="showLinkModal()">Associer Composition</button>
            <ModalAddTv v-if="showAddTv" @close="closeModal"></ModalAddTv>
            <ModifTv  v-if="showUpdate" v-bind:tv="tvToPass" @close="showUpdate = false">
            </ModifTv>
            <LinkTvCompo v-if="showLink" v-bind:tvs="this.tvs" v-bind:compositions="this.compositions" @close="showLink = false"></LinkTvCompo>
        </div>
    </main-layout>
</template>

<script>
    import MainLayout from '../layouts/Main.vue'
    import ModifTv from'../layouts/ModifTv.vue'
    import ModalAddTv from '../layouts/ModalAddTv.vue';
    import VLink from '../components/VLink.vue';
    import LinkTvCompo from '../layouts/LinkTvCompo.vue'

    let axios = require('axios');

    export default {
        components: {
            ModalAddTv,
            MainLayout,
            ModifTv,
            VLink,
            LinkTvCompo
        },
        data() {
            return {
                showAddTv: false,
                showUpdate: false,
                showLink: false,
                tvToPass: null,
                compositions: this.getCompositions(),
                tvs: this.getTv(),
                tv:{id:'', name:'', ipAdress:'', compositionId:''}
            }
        },
        methods: {
            closeModal() {
                console.log('modal closed');
                this.showAddTv = false;
                // TODO : Si tu veux aussi passer le modal update à false this.showUpdate = false;
                this.getTv();
            },
            getCompositions() {
                this.$http.get('http://localhost:8200/compositions')
                    .then(response => {
                        return response.json();
                    }).then(data => {
                    const resultArray = [];
                    for (let key in data) {
                        resultArray.push(data[key]);
                    }
                    this.compositions = resultArray;
                });
            },
            getTv(){
                axios.get('http://localhost:8089/tv')
                    .then(response=>{
                        this.tvs = response.data;
                    });
            },
            delTv: function(tv){
                axios.delete('http://localhost:8089/tv/' + tv.id);
                this.tvs.splice(this.tvs.indexOf(tv), 1);
            },
            modifyTv(tv){
                this.showUpdate = true;
                this.tvToPass = tv;
            },
            deleteCompo: function(c){
                this.$http.delete('http://localhost:8200/compositions/' + c.id)
                    .then(() => this.compositions.splice(this.compositions.indexOf(c), 1));
            },
            editCompo(c){
                // TODO : aller à l'écran de création de compo avec id
                this.$root.params = {id: c.id};
                this.$root.currentRoute = '/compoEdit';

            },
            showLinkModal(){
                this.showLink = true;
            }
        }
    }
</script>
