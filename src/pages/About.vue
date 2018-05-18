<template>
    <main-layout>
        <p>About page</p>
        <button class="btn btn-primary" @click="fetchData">Get Data</button>
        <ul class="list-group">
            <li class="list-group-item" v-for="c in compositions"> {{c.id}} </li>
        </ul>
    </main-layout>
</template>

<script>
    import MainLayout from '../layouts/Main.vue'

    export default {
        components: {
            MainLayout
        },
        data() {
            return {
                compositions: []
            }
        },
        methods: {
            fetchData() {
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
            }
        }
    }
</script>
