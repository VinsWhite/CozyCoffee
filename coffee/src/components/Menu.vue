<script setup>
import { ref } from 'vue'
import Basket from './Basket.vue'

const products = ref([ // prodotti inseriti
  { id: 1, title: 'Espresso Classico', description: 'Un espresso ricco e intenso, perfetto per iniziare la giornata.', actionBag: false },
  { id: 2, title: 'Cappuccino Crema', description: 'Cappuccino con una schiuma densa e cremosa, ideale per una pausa caffè.', actionBag: false },
  { id: 3, title: 'Latte Vaniglia', description: 'Latte fresco con un tocco dolce di vaniglia, ottimo per un momento di relax.', actionBag: false },
  { id: 4, title: 'Macchiato Caramello', description: 'Espresso con un delicato strato di schiuma e un leggero sapore di caramello.', actionBag: false },
  { id: 5, title: 'Mocha Cioccolato', description: 'Caffè espresso miscelato con cioccolato fondente e una crema dolce.', actionBag: false },
  { id: 6, title: 'Caffè Americano', description: 'Un caffè lungo e aromatico, perfetto per chi ama un gusto meno intenso.', actionBag: false },
  { id: 7, title: 'Affogato al Caffè', description: 'Gelato alla vaniglia "affogato" con una generosa dose di espresso caldo.', actionBag: false },
  { id: 8, title: 'Macchiato alla Menta', description: 'Espresso con una spruzzata di sciroppo di menta per un tocco fresco.', actionBag: false },
  { id: 9, title: 'Cappuccino Freddo', description: 'Versione fredda del cappuccino, ideale per le calde giornate estive.', actionBag: false },
  { id: 10, title: 'Latte Artigianale', description: 'Latte preparato con latte art decorativo, per una presentazione raffinata.', actionBag: false }
])

const cart = ref([])  // stato per il carrello

const toggleActionBag = (productId) => { // azione carrello
  const product = products.value.find(p => p.id === productId)
  if (product) {
    product.actionBag = !product.actionBag
    updateCart(product)
  }
}

const updateCart = (product) => { // funzione per aggiornare il carrello con le modifiche al prodotto
  const index = cart.value.findIndex(p => p.id === product.id)
  
  // fe il prodotto è selezionato per essere aggiunto al carrello (actionBag è true)
  if (product.actionBag) { 
    if (index === -1) { // se il prodotto non è già presente nel carrello
      cart.value.push({ ...product, quantity: 1 })
    } else {
      // se il prodotto è già nel carrello, incrementa la quantità di 1
      cart.value[index].quantity++
    }
  } else {
    // esegue questa parte solo se il prodotto è attualmente presente nel carrello
    if (index !== -1) {
      if (cart.value[index].quantity > 1) {
        // decrementa la quantità di 1
        cart.value[index].quantity--
      } else {
        // se la quantità è 1, rimuovi completamente il prodotto dal carrello
        cart.value.splice(index, 1)
      }
    }
  }
}

// gestione update del carrello
const handleCartUpdate = (updatedCart) => {
  cart.value = updatedCart
  products.value.forEach(product => {
    product.actionBag = cart.value.some(item => item.id === product.id)
  })
}
</script>

<template>
  <div class="mt-3 px-3">
    <div
      v-for="product in products"
      :key="product.id"
      class="shadow-sm p-2 border border-1 border-dark-subtle rounded-2 mb-3 position-relative"
    >
      <h3 class="fw-semibold">{{ product.title }}</h3>
      <p>{{ product.description }}</p>
      <i 
        @click="toggleActionBag(product.id)" 
        class="bi position-absolute top-0 end-0 p-2" 
        :class="{'bi-bag-check text-primary-emphasis': product.actionBag, 'bi-bag': !product.actionBag}"
      ></i>
    </div>
    <Basket :cart="cart" @updateCart="handleCartUpdate" />
  </div>
</template>
