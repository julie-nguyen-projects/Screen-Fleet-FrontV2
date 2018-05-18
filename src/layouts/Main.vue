<template>
    <div class="container">
        <div role="navigation">
            <ul class="nav nav-tabs">
                <li role="presentation">
                    <v-link href="/">Home</v-link>
                </li>
                <li role="presentation">
                    <v-link href="/about">About</v-link>
                </li>
                <li role="presentation">
                    <v-link href="/compo">Compositions</v-link>
                </li>
            </ul>
        </div>
        <slot></slot>
    </div>
</template>

<script>
    import VLink from '../components/VLink.vue'

    export default {
        components: {
            VLink
        }
    }
</script>

<style scoped>
    .container {
        margin: 0 auto;
        padding: 15px 30px;
        background: #f9f7f5;
    }
</style>
