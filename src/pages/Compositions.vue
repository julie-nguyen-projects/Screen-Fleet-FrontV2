<template>
    <main-layout>
        <div class="row" style="padding: 10px 20px">
            <h1>Ajouter / Editer une composition</h1>
        </div>

        <div class="row" style="min-height: 500px">
            <div class="col-md-8">
                Aperçu
            </div>
            <div class="col-md-4">
                Modules
            </div>
        </div>
        <div class="row">
            <button class="btn btn-primary" @click="addCompo()">OK</button>
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
                compo: {id: ''}
            }
        },
        methods: {
            addCompo() {
                console.log('addCompo');
                this.$http.post('http://localhost:8200/compositions', this.compo)
                    .then(() =>{
                        this.$root.currentRoute = '/';
                    });
            }
        }
    }
</script>
