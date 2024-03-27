<template>
  <div class="bg-[#60B7D4] p-4 shadow-md flex flex-col lg:flex-row items-center lg:space-x-4">
    <!-- User Profile Image -->
    <div class="absolute bottom-[70%] lg:mx-60 mx-auto">
      <img :src="user.image" alt="Profile Image" class="w-[35%] h-[35%]">
    </div>

    <div class="lg:relative lg:mt-8 lg:left-[25%] flex flex-col lg:flex-row items-center lg:mx-auto">
      <div class="mt-4 lg:mt-0 lg:mr-6">
        <p class="font-bold text-xl lg:text-2xl username">{{ user.name }}</p>
        <p class="text-gray-600">{{ user.lastOnline }}</p>
      </div>

      <div class="mt-4 lg:mt-8 lg:mt-0">
        <button @click="sendMessage"
          class="mx-2  lg:mx-4 bg-white text-[#60B7D4] hover:text-white  hover:border-white hover:border hover:bg-[#60B7D4] font-bold py-2 px-4 rounded focus:outline-none">
          <span class="pi pi-telegram"></span>
          Send Message
        </button>
        <button @click="addFriend"
          class="mx-2 lg:mx-4 border border-white hover:text-[#60B7D4] hover:border-0 hover:bg-white text-white font-bold py-2 px-4 rounded focus:outline-none">
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
  color: white;
}

@media (max-width: 768px) {
  .username {
    font-size: 20px;
  }
}
</style>
