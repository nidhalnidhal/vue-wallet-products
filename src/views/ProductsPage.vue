<template>
  <div class="p-6 max-w-4xl mx-auto">
    <h1 class="text-3xl font-bold mb-6">🛍️ Boutique de produits</h1>

    <!-- Badge panier -->
    <div class="mb-4">
      <button @click="showCart = !showCart" class="relative bg-blue-600 text-white px-4 py-2 rounded">
        Panier
        <span v-if="cartQuantity > 0" class="absolute -top-2 -right-2 bg-red-600 text-xs rounded-full px-2">
          {{ cartQuantity }}
        </span>
      </button>
    </div>

    <!-- Liste produits -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
      <div v-for="product in products" :key="product.id" class="border p-4 rounded shadow">
        <img :src="product.image" alt="" class="w-full h-36 object-cover mb-3 rounded" />
        <h2 class="font-semibold text-lg">{{ product.name }}</h2>
        <p class="text-gray-700 mb-2">{{ product.description }}</p>
        <p class="font-bold mb-3">${{ product.price.toFixed(2) }}</p>
        <button
          @click="addToCart(product)"
          class="bg-green-600 text-white px-4 py-2 rounded hover:bg-green-700"
        >
          Ajouter au panier
        </button>
      </div>
    </div>

    <!-- Panier -->
    <div v-if="showCart" class="mt-8 border p-4 rounded shadow bg-gray-50">
      <h2 class="text-2xl font-bold mb-4">🛒 Contenu du panier</h2>
      <p v-if="cartItems.length === 0">Le panier est vide.</p>

      <ul v-else>
        <li v-for="item in cartItems" :key="item.product.id" class="flex justify-between items-center mb-3">
          <div>
            <span class="font-semibold">{{ item.product.name }}</span>
            <span> x {{ item.quantity }}</span>
          </div>
          <div>
            <button
              @click="removeFromCart(item.product)"
              class="bg-red-600 text-white px-3 py-1 rounded hover:bg-red-700"
            >
              Supprimer
            </button>
          </div>
        </li>
      </ul>

      <div v-if="cartItems.length > 0" class="mt-4 font-bold">
        Total: ${{ cartTotal.toFixed(2) }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showCart: false,
      products: [
        {
          id: 1,
          name: "Laptop",
          price: 999.99,
          description: "A high-end laptop for professionals.",
          quantity: 10
        },
        {
          id: 2,
          name: "Smartphone",
          price: 599.99,
          description: "Latest generation smartphone.",
          quantity: 25
        },
        {
          id: 3,
          name: "Headphones",
          price: 199.99,
          description: "Noise-cancelling over-ear headphones.",
          quantity: 15
        }
      ],
      cart: [],
    }
  },
   async mounted() {
    try {
      const res = await fetch('http://localhost:8080/api/products') // adapt to your backend
      this.products = await res.json()
      console.log(this.products[0].name);
    } catch (err) {
      console.error('Erreur lors du chargement des produits :', err)
    }
  },
  computed: {
    cartQuantity() {
      return this.cart.reduce((total, item) => total + item.quantity, 0)
    },
    cartItems() {
      return this.cart
    },
    cartTotal() {
      return this.cart.reduce((total, item) => total + item.product.price * item.quantity, 0)
    },
  },
  methods: {
    addToCart(product) {
      const item = this.cart.find((i) => i.product.id === product.id)
      if (item) {
        item.quantity++
      } else {
        this.cart.push({ product, quantity: 1 })
      }
    },
    removeFromCart(product) {
      const index = this.cart.findIndex((i) => i.product.id === product.id)
      if (index !== -1) {
        this.cart.splice(index, 1)
      }
    },
  },
}
</script>
