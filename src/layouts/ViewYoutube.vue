<template>
	<img width="185" height="130" :src="this.getPicture" frameborder="0"/>
</template>

<script>
    export default {
        name: "ViewYoutube",
	    data() {
            return {
                videolink: this.link,
	            picturelink: ''
            }
	    },
	    props: ['link'],
	    computed:{
            getPicture: function(){
                this.picturelink = this.videolink.replace('www.youtube.com/watch?v=', 'img.youtube.com/vi/');
                this.picturelink = this.picturelink + '/0.jpg';
                console.log(this.picturelink);
                return this.picturelink;
            }
	    }
    }
</script>

<style scoped>

</style>