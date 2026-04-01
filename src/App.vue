<template>
    <Header
        v-bind:carrito ="carrito"
        v-bind:guitarraPortada="guitarraPortada"
        @restar-cantidad="restarCantidad"
        @sumar-cantidad="sumarCantidad"
        @agregar-carrito="agregarCarrito"
        @eliminar-producto="eliminarPorducto"
        @vaciar-carrito="vaciarCarro"
    />

    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección - Leiston Holguin</h2>

        <div class="row mt-5">
           <Guitarras
                v-for="guitarra in guitarras"   
                v-bind:guitarra = "guitarra"
                @agregar-carrito="agregarCarrito"
           /> 
        </div>
    </main>

    <Footer
    
    />


</template>

<script setup>

import {ref , onMounted , watch} from "vue"
import { db } from "./data/guitarras";
import Guitarras from "./components/Guitarras.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

    const guitarras = ref([])
    const carrito = ref([])
    const guitarraPortada = ref([])

    watch(carrito, () => {
        guardarLocalStore()
    } , {
        deep:true
    })

    onMounted(function(){
        guitarras.value = db
        guitarraPortada.value = db[3]
        const carritoStorage = localStorage.getItem('carrito')
        if(carritoStorage) {
            carrito.value = JSON.parse(carritoStorage)
        }

    })

    const guardarLocalStore = () => { 
        localStorage.setItem('carrito' , JSON.stringify(carrito.value))
    }

    const agregarCarrito = (guitarra) => {
        const existencia = carrito.value.findIndex(carrito => carrito.id === guitarra.id)

        if(existencia >= 0){
            carrito.value[existencia].cantidad++
        } else {
            guitarra.cantidad = 1
            carrito.value.push(guitarra)
        }
        guardarLocalStore()
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

    const eliminarPorducto = (id) => {
        carrito.value = carrito.value.filter(guitarra => guitarra.id !== id)
    }

    const vaciarCarro = () => {
        //carrito.value = carrito.value.filter(guitarra => guitarra.id === -1)
        carrito.value = []
    }

</script>

<style   scoped>

</style>