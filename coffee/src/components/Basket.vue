<script setup>
import { defineProps, defineEmits, ref } from 'vue'

const props = defineProps({
  cart: Array
})

const emit = defineEmits(['updateCart']) // update del carrello

const isVisible = ref(false)  // stato per gestire la visibilità del carrello

const adjustQuantity = (product, amount) => { // funzione per gestire la quantità di un prodotto nel carrello
  // indice del prodotto nel carrello
  const index = props.cart.findIndex(p => p.id === product.id)

  // verifica se il prodotto esiste nel carrello
  if (index !== -1) {
    const newQuantity = props.cart[index].quantity + amount // calcola la nuova quantità del prodotto 
    if (newQuantity > 0) { // se la nuova quantità è maggiore di zero, aggiorna la quantità del prodotto
      props.cart[index].quantity = newQuantity
    } else {
      // rimuovi il prodotto dal carrello se l'indice è 0
      props.cart.splice(index, 1)
    }

    emit('updateCart', props.cart) // update
  }
}


const clearCart = () => {
  props.cart.splice(0, props.cart.length)
  emit('updateCart', props.cart)
}

const toggleVisibility = () => {
  isVisible.value = !isVisible.value
  emit('updateCart', props.cart) // Emite aggiornamento
}
</script>

<template>
  <div class="basket-container position-fixed end-0 bottom-0 p-3">
    <div v-if="isVisible" class="cart-content border border-1 border-black rounded-2 p-2 bg-light">
      <h5 class="fw-semibold">Carrello</h5>
      <ul class="list-unstyled">
        <li v-for="item in props.cart" :key="item.id" class="d-flex align-items-center justify-content-between mb-2">
          <span class="me-2">{{ item.title }}:</span>
          <div class="d-flex align-items-center">
            <button @click="adjustQuantity(item, -1)" class="btn btn-sm btn-outline-secondary">-</button>
            <span class="mx-2">{{ item.quantity }}</span>
            <button @click="adjustQuantity(item, 1)" class="btn btn-sm btn-outline-secondary">+</button>
          </div>
        </li>
      </ul>
      <p class="border-top border-black pt-2">Prodotti in totale: {{ props.cart.reduce((total, item) => total + item.quantity, 0) }}</p>
      <button @click="clearCart" class="btn btn-danger w-100 mt-2 d-flex align-items-center justify-content-center">
        <i class="bi bi-trash me-2"></i> Svuota Carrello
      </button>
    </div>
    <div class="basket-icon position-fixed end-0 bottom-0 p-3" @click="toggleVisibility">
      <i class="bi bi-basket position-relative fs-3 bg-light p-2 rounded-3 shadow-sm"></i>
      <span v-if="props.cart.reduce((total, item) => total + item.quantity, 0)" class="badge bg-danger position-absolute top-0 end-0 rounded-pill">{{ props.cart.reduce((total, item) => total + item.quantity, 0) }}</span>
    </div>
  </div>
</template>
