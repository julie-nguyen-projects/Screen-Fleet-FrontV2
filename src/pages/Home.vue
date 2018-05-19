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
            <form>
                <p><label for="tvname">Tv name: </label><input class="champ" type="text" id="tvname" ref="tvname"/></p>
                <p><label for="ipAdress">Tv Ip: </label><input class="champ" type="text" id="ipAdress" ref="ipAdress"/></p>
                <p><label for="compo">Tv composition: </label><input class="champ" type="text" id="compo" ref="compo"/></p>
                <p><button @click.prevent="postTv()">Ajouter</button></p>
            </form>
            <ul class="list-group">
                <li class="list-group-item" v-for="tv in this.tvs"> {{ tv.name }} | {{ tv.ipAdress }} | {{ tv.compositionId }}
                    <button  id="show-update-modal" @click="showUpdateModal(tv)">Màj</button>
                    <button @click.prevent="delTv(tv)">Supprimer</button>
                </li>
            </ul>
            <button @click="showCreateTv = true">Ajouter une TV</button>
            <modal-add-tv v-if="showCreateTv" @close="closeAddTvModal()">
            </modal-add-tv>
            <modal v-if="showUpdate" :tvPassed="tvToPass" @close="showUpdate = false">
                <h3 slot="header">custom header : {{ tvPassed }}</h3>
            </modal>
        </div>
    </main-layout>
</template>

<script>
    import MainLayout from '../layouts/Main.vue'
    import Modal from'../layouts/Modal.vue'
    import ModalAddTv from "../layouts/ModalAddTv";

    let axios = require('axios');

    export default {
        components: {
            ModalAddTv,
            MainLayout,
            Modal
        },
        data() {
            return {
                showCreateTv: false,
                showUpdate: false,
                tvToPass: null,
                compositions: this.getCompositions(),
                tvs: this.getTv(),
                tv:{id:'', name:'', ipAdress:'', compositionId:''}
            }
        },
        methods: {
            showUpdateModal(tv) {
                console.log(tv.name);
                this.showUpdate = true;
                this.tvToPass = tv;
            },
            closeAddTvModal() {
              this.showCreateTv = false;
              this.postTv();
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
                        this.tvs = response.data;
                    });
            },
            delTv: function(tv){
                axios.delete('http://localhost:8089/tv/' + tv.id);
                this.tvs.splice(this.tvs.indexOf(tv), 1);
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
                        this.tvs.push(response.data);
                    });
            }
        }
    }
</script>
