<template>
    <transition name="modal">
        <div class="modal-mask">
            <div class="modal-wrapper">
                <div class="modal-container">

                    <div class="modal-header">
                        <slot name="header">
                            Ajouter une TV
                        </slot>
                    </div>

                    <div class="modal-body">
                        <slot name="body">
                            <form>
                                <p><label for="tvname">Tv name: </label><input class="champ" type="text" id="tvname" ref="tvname"/></p>
                                <p><label for="ipAdress">Tv Ip: </label><input class="champ" type="text" id="ipAdress" ref="ipAdress"/></p>
                                <p><label for="compo">Tv composition: </label><input class="champ" type="text" id="compo" ref="compo"/></p>
                            </form>
                        </slot>
                    </div>

                    <div class="modal-footer">
                        <slot name="footer">
                            <button class="modal-default-button" @click="postTv()">
                                OK
                            </button>
                        </slot>
                    </div>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>
    export default {
        name: "ModalAddTv",
        data() {
            return {
                tv: {id: '', name: '', ipAdress: '', compositionId: ''}
            }
        }
    }
</script>

<style scoped>

</style>