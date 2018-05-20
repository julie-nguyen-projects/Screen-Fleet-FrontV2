<template>
    <transition name="modal">
        <div class="modal-mask">
            <div class="modal-wrapper">
                <div class="modal-container">

                    <div class="modal-header">
                        <slot name="header">
                            <h1>Ajouter Média</h1>
                        </slot>
                    </div>

                    <div class="modal-body">

                        <form>
                            <div class="row">
                                <div class="col-xs-12 col-sm-8 col-md-12 col-sm-offset-2  col-md-offset-3">
                                    <div class="form-group">
                                        <label>Nom du Média</label>
                                        <input
                                                type="text"
                                                id="name"
                                                class="form-control" v-model="name">
                                    </div>
                                    <div class="form-group">
                                        <label> URL </label>
                                        <input
                                                type="url"
                                                id="path"
                                                class="form-control" v-model="path">
                                    </div>
                                    <div class="form-group">
                                        <label>Type du Média</label>
                                        <input
                                                type="number"
                                                id="type"
                                                class="form-control" min="0" max="2" v-model="mediaType">
                                    </div>
                                </div>
                            </div>
                            <hr>
                            <div class="row">
                                <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3" style="float: right">
                                    <button
                                            class="modal-default-button" @click.prevent="$emit('close')">Annuler
                                    </button>
                                    <button
                                            class="modal-default-button" @click.prevent="post">Poster
                                    </button>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </transition>

</template>

<script>
    import axios from 'axios'
    import { compoBus } from '../main.js'
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
                if (this.name != '' && this.path != '' && this.mediaType != '') {
                    const formData = {
                        name: this.name,
                        path: this.path,
                        mediaType: this.mediaType
                    }
                    axios.post('http://localhost:8100/resource-media/', formData).then(res => {
                        console.log(res)
                        this.medias.push(res.data)
                        $('form :input').val('');
                        this.$emit('close');
                        compoBus.$emit('postNewMedia',res.data)
                    }).catch(err => {
                        console.log(err)
                    })
                } else {
                    event.preventDefault()
                    if (this.name == ''){
                        alert('Please insert name')
                    } else if (this.path == '') {
                        alert('Please insert path')
                    } else {
                        alert('Please insert type')
                    }
                }
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
        }
    }
</script>


<style scoped>
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
    button.red {
        background: -webkit-linear-gradient(top, #DD4B39, #D14836);
        background: -moz-linear-gradient(top, #DD4B39, #D14836);
        background: -ms-linear-gradient(top, #DD4B39, #D14836);
        border: 1px solid #DD4B39;
        color: white;
        text-shadow: 0 1px 0 #C04131;
        height: auto;
        width: auto;
    }
    button.red:hover {
        background: -webkit-linear-gradient(top, #DD4B39, #C53727);
        background: -moz-linear-gradient(top, #DD4B39, #C53727);
        background: -ms-linear-gradient(top, #DD4B39, #C53727);
        border: 1px solid #AF301F;
    }

    button.red:active {
        box-shadow: inset 0 1px 1px rgba(0,0,0,0.2);
        background: -webkit-linear-gradient(top, #D74736, #AD2719);
        background: -moz-linear-gradient(top, #D74736, #AD2719);
        background: -ms-linear-gradient(top, #D74736, #AD2719);
    }

    label {
        color: #B4886B;
        font-weight: bold;
        display: block;
        float: left;
    }
    label:after { content: ": " }

    h1 { color: #7c795d;
        font-family: 'Trocchi',
        serif; font-size: 45px;
        font-weight: normal;
        line-height: 48px;
        margin: 0;
    }

    .postShow {
        -moz-box-shadow: 0px 1px 0px 0px #f0f7fa;
        -webkit-box-shadow: 0px 1px 0px 0px #f0f7fa;
        box-shadow: 0px 1px 0px 0px #f0f7fa;
        background:-webkit-gradient(linear, left top, left bottom, color-stop(0.05, #33bdef), color-stop(1, #019ad2));
        background:-moz-linear-gradient(top, #33bdef 5%, #019ad2 100%);
        background:-webkit-linear-gradient(top, #33bdef 5%, #019ad2 100%);
        background:-o-linear-gradient(top, #33bdef 5%, #019ad2 100%);
        background:-ms-linear-gradient(top, #33bdef 5%, #019ad2 100%);
        background:linear-gradient(to bottom, #33bdef 5%, #019ad2 100%);
        filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#33bdef', endColorstr='#019ad2',GradientType=0);
        background-color:#33bdef;
        -moz-border-radius:6px;
        -webkit-border-radius:6px;
        border-radius:6px;
        border:1px solid #057fd0;
        display:inline-block;
        cursor:pointer;
        color:#ffffff;
        font-family:Arial;
        font-size:15px;
        font-weight:bold;
        padding:6px 24px;
        text-decoration:none;
        text-shadow:0px -1px 0px #5b6178;
    }
    .postShow:hover {
        background:-webkit-gradient(linear, left top, left bottom, color-stop(0.05, #019ad2), color-stop(1, #33bdef));
        background:-moz-linear-gradient(top, #019ad2 5%, #33bdef 100%);
        background:-webkit-linear-gradient(top, #019ad2 5%, #33bdef 100%);
        background:-o-linear-gradient(top, #019ad2 5%, #33bdef 100%);
        background:-ms-linear-gradient(top, #019ad2 5%, #33bdef 100%);
        background:linear-gradient(to bottom, #019ad2 5%, #33bdef 100%);
        filter:progid:DXImageTransform.Microsoft.gradient(startColorstr='#019ad2', endColorstr='#33bdef',GradientType=0);
        background-color:#019ad2;
    }
    .postShow:active {
        position:relative;
        top:1px;
    }

    .modal-mask {
        position: fixed;
        z-index: 9998;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, .5);
        display: table;
        transition: opacity .3s ease;
    }

    .modal-wrapper {
        display: table-cell;
        vertical-align: middle;
    }

    .modal-container {
        width: 400px;
        margin: 0px auto;
        padding: 20px 30px;
        background-color: #fff;
        border-radius: 2px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
        transition: all .3s ease;
        font-family: Helvetica, Arial, sans-serif;
    }

    .modal-header h3 {
        margin-top: 0;
        color: #42b983;
    }

    .modal-body {
        margin: 20px 0;
    }

    .modal-default-button {
        float: right;
    }

    /*
     * The following styles are auto-applied to elements with
     * transition="modal" when their visibility is toggled
     * by Vue.js.
     *
     * You can easily play with the modal transition by editing
     * these styles.
     */

    .modal-enter {
        opacity: 0;
    }

    .modal-leave-active {
        opacity: 0;
    }

    .modal-enter .modal-container,
    .modal-leave-active .modal-container {
        -webkit-transform: scale(1.1);
        transform: scale(1.1);
    }
</style>