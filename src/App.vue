<script setup>
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
import Guitarras from "./components/Guitarra.vue";
import { ref, reactive, onMounted, watch } from "vue";
import { db } from "./data/guitarras";

const guitarras = ref([]);
const carrito = ref([]);
const guitarra = db[3]
watch(carrito, ()=>{
    guardarLocalStorage()
    console.log('algo Cambio')
},{

    deep:true
})
onMounted(() => {

  guitarras.value = db;
    const carritoStorage = localStorage.getItem('carrito')
    if(carritoStorage){
        carrito.value = JSON.parse(carritoStorage)


    }

});

const agregarCarrito = (guitarra) => {
    
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)

    console.log(existeCarrito)
    if(existeCarrito >= 0){

    console.log("existe en el carrito")
    
    carrito.value[existeCarrito].cantidad++
                
    }else{
        guitarra.cantidad = 1
        carrito.value.push(guitarra)
    }



};

const decrementarCantidad = (id) => {
console.log(
    "decrement ",id
)
const index = carrito.value.findIndex(producto => producto.id === id)

if(carrito.value[index].cantidad <=1) return
carrito.value[index].cantidad--


}

const incrementarCantidad = (id)=>{

const index = carrito.value.findIndex(producto => producto.id === id)
if(carrito.value[index].cantidad >=5 ) return
carrito.value[index].cantidad++

console.log("increment ",id)


}

const eliminarProducto =(id) => {

    carrito.value = carrito.value.filter(producto => producto.id !== id)
    console.log("Eliminar Producto",id)
}


const vaciarCarrito = ()=>{

    const carritoStorage = ()=> localStorage.clear()
    carritoStorage()
    carrito.value = []

}

// Cart to local storage

const guardarLocalStorage = ()=>{

    localStorage.setItem('carrito',JSON.stringify(carrito.value))
}
</script>

<template>
  <Header 
  :carrito="carrito"
  :guitarra ="guitarra"
  @incrementar-cantidad = "incrementarCantidad"
  @decrementar-cantidad = "decrementarCantidad"
  @eliminar-producto = "eliminarProducto"
  @vaciar-carrito = "vaciarCarrito"
  @agregar-carrito="agregarCarrito"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <guitarras
        v-for="guitarra in guitarras"
        :guitarra="guitarra"
        @agregar-carrito="agregarCarrito"

      />
    </div>
  </main>
  <Footer />
</template>
