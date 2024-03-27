<template>
  <div class="text-black mx-60">
    <div class="flex justify-between mx-auto">
      <!-- Logo -->
      <div class="flex items-center">
        <img src="../../public/Logo.png" alt="Logo" class="h-20 mr-4">
      </div>
      <!-- Icons -->
      <div class="flex items-center space-x-4">
        <button @click="handleIconClick('icon1')" class="focus:outline-none">
          <span class="pi pi-bell"></span>
        </button>
        <button @click="handleIconClick('icon2')" class="focus:outline-none">
          <span class="pi pi-cog"></span>
        </button>
        <button @click="handleIconClick('icon3')" class="focus:outline-none">
          <span class="pi pi-sign-out"></span>
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import 'primeicons/primeicons.css'
export default {
  methods: {
    handleIconClick(icon) {
      console.log('Icon clicked:', icon);
    }
  }
}
</script>