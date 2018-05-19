<template>
	<transition name="modal">
		<div class="modal-mask">
			<div class="modal-wrapper">
				<div class="modal-container">
					<form>
						<p><label for="tvnameModal">Tv name: </label><input class="champ" type="text" id="tvnameModal" ref="tvnameModal" :value='tv.name'/></p>
						<p><label for="ipAdressModal">Tv Ip: </label><input class="champ" type="text" id="ipAdressModal" ref="ipAdressModal" :value='tv.ipAdress'/></p>
						<p><label for="compoModal">Tv composition: </label><input class="champ" type="text" id="compoModal" ref="compoModal" :value='tv.compositionId'/></p>
						<p><button @click.prevent="putTv(tv.id)">Modifier</button><button class="modal-default-button" @click="$emit('close')">Annuler</button></p>
					</form>
				</div>
			</div>
		</div>
	</transition>
</template>

<script>
    let axios = require('axios');
    
    export default {
        name: 'ModifTv',
	    props: ['tv'],
        methods: {
            closeModal() {
                console.log('closed');
            },
	        putTv(){
                if (this.$refs.tvnameModal.value === '' ){
                    alert("Tv name can not be empty");
                    return false;
                }else if (this.$refs.ipAdressModal.value === '' ){
                    alert("Tv Ip can not be empty");
                    return false;
                }
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