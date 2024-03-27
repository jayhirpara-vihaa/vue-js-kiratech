<template>
  <div class="bg-[#60B7D4] p-4 shadow-md flex items-center space-x-4">
    <!-- User Profile Image -->
    <div class="absolute bottom-[75%] mx-60">
      <img :src="user.image" alt="Profile Image" class="w-[35%] h-[35%]">
    </div>

    <div class="relative left-[20%] flex">
      <div class="mt-7">
        <p class="font-bold username">{{ user.name }}</p>
        <p class="text-gray-600">{{ user.lastOnline }}</p>
      </div>

      <div class="mt-12">
        <button @click="sendMessage"
          class="mx-4 bg-white text-[#60B7D4] hover:text-white  hover:border-white hover:border hover:bg-[#60B7D4] font-bold py-2 px-4 rounded focus:outline-none">
          <span class="pi pi-telegram"></span>
          Send Message
        </button>
        <button @click="addFriend"
          class="border border-white hover:text-[#60B7D4] hover:border-0 hover:bg-white text-white font-bold py-2 px-4 rounded focus:outline-none">
          <span class="pi pi-plus"></span>

          Add Friend
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    user: {
      type: Object,
      required: true
    }
  },
  methods: {
    sendMessage() {
      console.log('Send message button clicked');
    },
    addFriend() {
      console.log('add friend button clicked');
    }
  }
}
</script>

<style scoped>
.username {
  font-size: 26px;
  color: white
}
</style>
