<template>
  <Speanner v-if="loading"/>

  <div class="trabajo" v-else>

    <h1 v-text="name"></h1>
    <button v-on:click="handleClick()">boton</button>
    <button v-on:click="vue()">return</button>
    <ButtonCounter :buttonText="'-'" @lectura="lectura" />
    <!-- <ButtonCounter>hola mundo</ButtonCounter> -->

    <div class="Mouse">
      <h2>Calculadora</h2>
      <h3 v-text="Total"></h3>
      
      <div class="subtotal">
        <h3 v-text="`${signo} ${subTotal}`"></h3>
      </div>
      
      <div class="reset">
        <ButtonCounter :buttonText="'C'" @lectura="handleClick5" />
        <ButtonCounter :buttonText="'CE'" @lectura="handleClick5" />
      </div>
      
      <div class="Calcu">
        <ButtonCounter v-for="(dato, index) in calculo" :key="index" :buttonText="dato" @lectura="handleClick5" />
      </div>

    </div>

    <div class="Post">
      <h2 v-text="`Mis Post Favorito: ${favorite}`"/>
      <div class="botones">
        <ButtonCounter :buttonText="'Ant'" 
        @lectura="MostrarData" 
        :clase="'boton-Post'" 
        :paso="userId === 1"/>
        <!-- <ButtonCounter :buttonText="'Middle'" @lectura="MostrarData" :clase="'boton-Post'"/> -->
        <ButtonCounter :buttonText="'Next'" 
        @lectura="MostrarData" 
        :clase="'boton-Post'"
        :paso="userId === tamanio"/>
      </div>
        
      <template v-for="(dato, index) in Post" :key="index">
        <BlogPost v-if="dato.userId == userId"
        :title="dato.title" 
        :id="dato.id" 
        :body="dato.body" 
        :color="dato.color"
        @Favorito="Favorito"/>
      </template>
    </div>
    
    <MDelete :Numero="10000" />
  </div>

</template>
<!--  -->
<style>
  @import 'assets/styles/app.css';
</style>


<script>
  import { metodos } from './assets/js/bundle.js';
  import { ref, onMounted, computed } from 'vue';
  import ButtonCounter from './components/ButtonCounter.vue';
  import BlogPost from './components/BlogPost.vue';
  import MDelete from './components/MillonDelete.vue';
  import Speanner from './components/LoadingSpinner.vue';
  // const name = ref(metodos.mensaje3());

  // onMounted(() => {
  //   // console.log(metodos.listaNumeros.includes(this.valor));
  //   await Cargar();
  // });

  export default {
    data() {
      return {
        name: ref(metodos.mensaje3()),
        calculo: ref(metodos.arrayCal()),
        Total: 0,
        subTotal: 0,
        signo: '',
        Post: [],
        favorite: '',
        userId: 1,
        tamanio: 0,
        loading: ref(true)
      };
    },
    computed: {

    },
    methods: {
      handleClick() {
        this.name = metodos.mensaje();
      }, 
      vue () {
        this.name = metodos.mensaje3();
      },
      lectura(texto) {
        metodos.lectura(texto);
      },
      handleClick5(texto) {
        const { Total, subTotal, signo } = metodos.calculadora(this.Total, this.subTotal, this.signo, texto);
        this.Total = Total;
        this.subTotal = subTotal;
        this.signo = signo;
      },
      Favorito(texto) {
        this.favorite = texto;
      },
      async Cargar() {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/posts');
          this.Post = await response.json();
          // this.Post = data;
          this.tamanio = this.Post[this.Post.length-1].userId;
        } catch (error) {
          console.error(error);
        } finally {
          setTimeout(() => {
            this.loading = false;
          }, 500);
        }
      },
      MostrarData(texto) {
        switch(texto) {
          case 'Ant':
            if(this.userId!=1) this.userId--;
            break;
          case 'Next':
            if(this.tamanio>this.userId) this.userId++;
            break;
          default:
            console.log("texto desconocido");
            break;
        }
      },
      completar(paso) {
        console.log("hola mundo")
        this.millos = paso;
      }
    },
    async created() {
      // console.log(this.name);
      // console.log(metodos.Post2);
      await this.Cargar();
    },
    // async onMounted() {
    //   console.log("El componente ha sido montado");
    //   await this.Cargar();
    //   // Puedes agregar aquí cualquier lógica que desees ejecutar después de que el componente haya sido montado.
    // },
    components: {
      ButtonCounter,
      BlogPost,
      MDelete,
      Speanner
    }
  };
</script>

<!-- <script setup>
import { ref } from 'vue';
import { metodos } from './assets/js/bundle.js';
import ButtonCounter from './components/ButtonCounter.vue';

const name = ref(metodos.mensaje3());

const handleClick = () => {
  name.value = metodos.mensaje();
};

const vue = () => {
  name.value = metodos.mensaje3();
};

// onMounted(() => {
//   console.log(name.value);
// });
</script> -->