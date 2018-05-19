<template>
    <main-layout>
        <div class="row" style="padding: 10px 20px">
            <h1>Editer une composition (id : {{this.$root.params.id}})</h1>
        </div>

        <div class="row" style="min-height: 500px">
            <div class="col-md-8">
                <p>Aperçu de la compo d'id : {{compo.id}}</p>
                <p>{{compo}}</p>
            </div>
            <div class="col-md-4">
                Modules
            </div>
        </div>
        <div class="row">
            <button class="btn btn-primary" @click="editCompo">OK</button>
            <v-link class="btn btn-secondary" href="/">Annuler</v-link>
        </div>
    </main-layout>
</template>

<script>
    import MainLayout from '../layouts/Main.vue';
    import VLink from '../components/VLink.vue';

    export default {
        components: {
            MainLayout,
            VLink
        },
        data() {
            return {
                compo: this.getCompoById(this.$root.params.id)
            }
        },
        methods: {
            getCompoById(id) {
                this.$http.get('http://localhost:8200/compositions/' + id)
                    .then(response => {
                        return response.json();
                    }).then(data => this.compo = data);
            },
            editCompo() {
                this.$http.put('http://localhost:8200/compositions', this.compo)
                    .then(() =>{
                        this.$root.currentRoute = '/';
                    });
            }
        }
    }
</script>
