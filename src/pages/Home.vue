<template>
    <main-layout>
        <div class="col-md-6">
            <h1>Compositions</h1>
            <ul class="list-group">
                <li class="list-group-item" v-for="c in this.compositions"> {{c.id}} </li>
            </ul>
        </div>

        <div class="col-md-6">
            <h1>Télévisions</h1>
            <p><button @click.prevent="getTv()">Actualiser</button></p>
            <form>
                <p><label for="tvname">Tv name: </label><input class="champ" type="text" id="tvname" ref="tvname"/></p>
                <p><label for="ipAdress">Tv Ip: </label><input class="champ" type="text" id="ipAdress" ref="ipAdress"/></p>
                <p><label for="compo">Tv composition: </label><input class="champ" type="text" id="compo" ref="compo"/></p>
                <p><button @click.prevent="postTv()">Ajouter</button></p>
            </form>
            <ul>
                <li v-for="tv in this.Tvs">
                    {{ tv.name }} | {{ tv.ipAdress }} | {{ tv.compositionId }}
                    <button id="show-modal" @click.prevent="modifyTv(tv)">Modifier</button>
                    <button @click.prevent="delTv(tv)">Supprimer</button>
                </li>
            </ul>
            <ModifTv  v-if="showUpdate" v-bind:tv="tvToPass" @close="showUpdate = false">
            </ModifTv>
        </div>
    </main-layout>
</template>

<script>
    import MainLayout from '../layouts/Main.vue'
    import ModifTv from'../layouts/ModifTv.vue'

    let axios = require('axios');

    export default {
        components: {
            MainLayout,
            ModifTv
        },
        data() {
            return {
                showUpdate: false,
                tvToPass: null,
                compositions: this.getCompositions(),
                Tvs: this.getTv(),
                tv:{id:'', name:'', ipAdress:'', compositionId:''}
            }
        },
        methods: {
            showUpdateModal(tv) {
                console.log(tv.name);
                this.showUpdate = true;
                this.tvToPass = tv;
            },
            getCompositions() {
                this.$http.get('http://localhost:8200/compositions')
                    .then(response => {
                        console.log(response.json());
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
                        this.Tvs = response.data;
                    });
            },
            delTv: function(tv){
                axios.delete('http://localhost:8089/tv/' + tv.id);
                this.Tvs.splice(this.Tvs.indexOf(tv), 1);
            },
            postTv(){
                if (this.$refs.tvname.value === '' ){
                    alert("Tv name can not be empty");
                    return false;
                }else if (this.$refs.ipAdress.value === '' ){
                    alert("Tv Ip can not be empty");
                    return false;
                }

                this.tv.ipAdress = this.$refs.ipAdress.value;
                this.tv.name = this.$refs.tvname.value;
                this.tv.compositionId = this.$refs.compo.value;

                axios.post('http://localhost:8089/tv', this.tv)
                    .then(response =>{
                        this.Tvs.push(response.data);
                    });
            },
            modifyTv(tv){
                //console.log(tv.name);
                this.showUpdate = true;
                this.tvToPass = tv;
            }
        }
    }
</script>
