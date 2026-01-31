<template>
    <headers
        v-bind:carrito ="carrito"
        @restar-cantidad="restarCantidad"
        @sumar-cantidad="sumarCantidad"
    />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
           <guitarra
             v-for="guitarra in guitarras"   
             v-bind:guitarra = "guitarra"
             @agregar-carrito="agregarCarrito"
           /> 
        </div>
    </main>

    <footers
    
    />


</template>

<script setup>

import {ref , reactive , onMounted} from "vue"
import { db } from "./data/guitarras";
import guitarra from "./components/guitarra.vue";
import headers from "./components/headers.vue";
import footers from "./components/footers.vue";

//const state = reactive({
//    guitarra : db,
//})


    const guitarras = ref([])
    const carrito = ref([])

    onMounted(function(){
        guitarras.value = db
    })


    const agregarCarrito = (guitarra) => {
        const existencia = carrito.value.findIndex(carrito => carrito.id === guitarra.id)

        if(existencia >= 0){
            carrito.value[existencia].cantidad++
        } else {
            guitarra.cantidad = 1
            carrito.value.push(guitarra)
        }

    }

    const restarCantidad = (id) => {
        const posicionResta = carrito.value.findIndex(carrito => carrito.id === id)
        if(carrito.value[posicionResta].cantidad <= 1) return;
        carrito.value[posicionResta].cantidad--
    }

    const sumarCantidad = (id) => {
        const posicionSuma = carrito.value.findIndex(carrito => carrito.id === id)
        if(carrito.value[posicionSuma].cantidad >= 5) return;
        carrito.value[posicionSuma].cantidad++

    }
</script>

<style   scoped>

</style>