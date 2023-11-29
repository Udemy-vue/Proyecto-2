<template>
	<div class="caja-1">
		<div class="caja-body">
			<h5 class="card-title" v-text="`${id} - ${title}`" :style="{ color }"/>
			<p class="texto_justificado" v-text="body"/>
			<button v-on:click="emitFavorito" v-text="`Favorito`"></button>
		</div>

	</div>
</template>

<script>
	import {ref} from 'vue';

	export default {
		
		// props: ['title', 'id', 'lorem', 'color'],
		props: {
			userId: Number,
	    title: String,
	    id: Number,
	    body: {
	      type: String,
	      default: "sin descripcion"
	    },
	    color: String
	  },
	  methods: {
	  	emitFavorito(){
	  		this.$emit('Favorito', this.title);
	  	}
	  },
		created() {
			// console.log(this.title);
			// console.log(this.id);
		}
	}
</script>