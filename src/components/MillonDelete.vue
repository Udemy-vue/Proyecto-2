<template>

	<Speanner v-if="paso"/>
	<div v-else class="prueba">
		<div class="titulo">
			<h2 class="texto_centrado mayuscula">Prueba de desarrollo</h2>
			<p class="texto_justificado">Desarrollo de logica, para eto se daran dos listas, la primera se generara una lista de {{ Numero }} de numeros desde el 1 hasta {{ Numero }}, y la segunda lista es una lista desde 1 hasta {{ Numero }} con solo los numeros impares. </p>
			<p class="texto_justificado">El juego es retirar los numeros en la primera lista, utilizando los numeros de la segunda lista que salen.</p>
		</div>
		<h2 style="margin:0">lista de numeros</h2>
		
		<div class="lista">
			<h3 v-for="(num, index) in Total" :key="index" v-text="num" />
		</div>
		
		<h2 style="margin:0">lista de numero impares</h2>

		<div class="lista">
			<h3 v-for="(num, index) in Impar" :key="index" v-text="num" />
		</div>

		<h2 style="margin:0">lista de numero pares</h2>

		<div class="lista">
			<h3 v-for="(num, index) in Par" :key="index" v-text="num" />
		</div>

		<h2 style="margin:0">El tiempo de ejecución fue de {{tiempo}} segundos.</h2>
	</div>
	<!-- <h2>hola</h2> -->
</template>

<script>
	import { ref, onMounted, computed } from 'vue';
	import { prueba } from '@/assets/js/bundle.js';
  import Speanner from './LoadingSpinner.vue';

	export default {
		props: ['Numero'],

		data() {
			return {
				Total: Array.from({ length: this.Numero }, (_, index) => index + 1),
				Impar: Array.from({ length: this.Numero/2 }, (_, index) => index * 2 + 1),
				Par: [],
				paso: ref(true),
				tiempo: 0.0
			};
		},
		methods: {
			async realizarCalculo() {
		    // Simulación de la operación asíncrona
		    await new Promise(resolve => setTimeout(resolve, 1000));

		    // Llamada a la función asíncrona
		    this.Par = await prueba.calcular(this.Total, this.Impar);
		  }
		},
		async created() {
			console.log("inicio de la filtracion");

    	const tiempoInicio = new Date();

    	// Esperar a que se complete la operación antes de continuar
		  await this.realizarCalculo();

		  const tiempoFin = new Date();
		  this.tiempo = (tiempoFin - tiempoInicio) / 1000;
		  this.paso = false;

			// this.Par = prueba.calcular(this.Total, this.Impar);
    	// const tiempoFin = new Date();
    	// console.log(`El tiempo de ejecución fue de ${(tiempoFin - tiempoInicio) / 1000} segundos.`);
    	// this.paso = false;
		},
		components: {
			Speanner
		},
		async onMounted() {
      
      // Puedes agregar aquí cualquier lógica que desees ejecutar después de que el componente haya sido montado.
    }
	}
</script>