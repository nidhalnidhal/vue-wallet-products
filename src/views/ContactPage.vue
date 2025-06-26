<template>
  <div class="max-w-lg mx-auto p-6">
    <h1 class="text-3xl font-bold mb-6">Contactez-nous</h1>
    <form @submit.prevent="handleSubmit" novalidate>
      <div class="mb-4">
        <label class="block mb-1 font-semibold" for="email">Email :</label>
        <input
          id="email"
          v-model="email"
          type="email"
          required
          class="w-full border px-3 py-2 rounded"
        />
        <p v-if="errors.email" class="text-red-600 text-sm mt-1">{{ errors.email }}</p>
      </div>

      <div class="mb-4">
        <label class="block mb-1 font-semibold" for="message">Message :</label>
        <textarea
          id="message"
          v-model="message"
          required
          maxlength="300"
          rows="5"
          class="w-full border px-3 py-2 rounded"
        ></textarea>
        <p v-if="errors.message" class="text-red-600 text-sm mt-1">{{ errors.message }}</p>
      </div>

      <button
        type="submit"
        class="bg-blue-600 text-white px-5 py-2 rounded hover:bg-blue-700"
      >
        Envoyer
      </button>

      <p v-if="successMessage" class="mt-4 text-green-600 font-semibold">{{ successMessage }}</p>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      message: '',
      errors: {},
      successMessage: '',
    }
  },
  methods: {
    validate() {
      this.errors = {}
      if (!this.email) {
        this.errors.email = 'L\'email est requis.'
      } else if (!this.validEmail(this.email)) {
        this.errors.email = 'L\'email n\'est pas valide.'
      }
      if (!this.message) {
        this.errors.message = 'Le message est requis.'
      } else if (this.message.length > 300) {
        this.errors.message = 'Le message doit contenir moins de 300 caractères.'
      }
      return Object.keys(this.errors).length === 0
    },
    validEmail(email) {
      const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
      return re.test(email)
    },
    handleSubmit() {
      if (this.validate()) {
        // Simule envoi — ici tu peux faire appel à un backend si besoin
        this.successMessage = 'Demande de contact envoyée avec succès.'
        this.email = ''
        this.message = ''
      }
    },
  },
}
</script>
