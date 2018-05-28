<template>
	<transition name="modal">
		<div class="modal-mask">
			<div class="modal-wrapper">
				<div class="modal-container">
					<form>
						<p>
							<label>Choix TV: </label>
							<select>
								<option value="">--TV--</option>
								<option v-for="tv in tvs" :value="tv.id" ref="tvChoice" @click.prevent=setTv(tv)>{{ tv.name }}</option>
							</select>
						</p>
						<p>
							<label>Choix Composition: </label>
							<select>
								<option value="">--Composition--</option>
								<option v-for="compo in compositions" :value="compo.id" ref="compoChoice"  @click.prevent=setCompo(compo)>{{ compo.id }}</option>
							</select>
						</p>
						<p><button @click.prevent="putTv()"  @click="$emit('close')">Lier</button><button class="modal-default-button" @click="$emit('close')">Annuler</button></p>
					</form>
				</div>
			</div>
		</div>
	</transition>
</template>

<script>
    let axios = require('axios');
    export default {
        name: "LinkTvCompo",
        props: ['tvs', 'compositions'],
        data(){
            return{
                tv:{id:'', name:'', ipAdress:'', compositionId:''},
	            compo:{id:''}
            }
        },
        methods: {
            closeModal(){
                console.log('closed');
            },
            putTv(){
                console.log(this.$refs.compoChoice.value);

                this.tv.compositionId = this.compo.id;
                axios.put('http://localhost:8089/tv/'+this.tv.id, this.tv);
            },
            setTv(aTv){
                this.tv = aTv;
            },
	        setCompo(aCompo){
                this.compo = aCompo;
	        }
        }
    }
</script>

<style scoped>
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