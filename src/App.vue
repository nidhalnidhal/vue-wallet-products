<template>
  <div class="flex min-h-screen">
    <nav class="w-48 bg-gray-100 p-6">
      <ul>
        <li class="mb-4">
          <router-link to="/" class="text-blue-700 hover:underline">Produits</router-link>
        </li>
        <li>
          <router-link to="/contact" class="text-blue-700 hover:underline">Contact</router-link>
        </li>
      </ul>
    </nav>

    <main class="flex-1 p-6 bg-white">
      <router-view />
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
}
</script>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
}
</style>
