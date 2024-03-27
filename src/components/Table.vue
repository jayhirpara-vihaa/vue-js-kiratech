<template>
  <div v-if="isLoading" class="loader"></div>
  <div v-if="!isLoading" class="mt-16">
    <!-- header -->
    <div class="mx-60 rounded-md bg-white ">
      <div>
        <div class="flex justify-between w-[100%]">

          <div class="  w-[15%] text-zinc-400 text-left ml-5">Date</div>
          <div class="w-[30%] text-zinc-400 text-left">Name</div>

          <div class="w-[11%]  text-zinc-400 text-left">Gender</div>
          <div class="w-[11%] text-zinc-400 text-left">Country</div>


          <div class="w-[30%] text-zinc-400 text-center">Email</div>
        </div>
      </div>
    </div>

    
    <!-- data -->
    <div class="mx-60 rounded-md bg-white shadow-md my-3 ">
      <div v-for="(user, index) in users" :key="index" class="card hover:border-2 hover:border-[#60B7D4]">
        <div class="flex justify-between w-[100%] h-7 my-1">

          <div class="text-sm text-zinc-400 w-[15%] text-left">{{ format_date(user.date) }}</div>
          <div class="font-medium hover:font-medium hover:text-[#60B7D4] cursor-pointer w-[30%] text-left"
            @click="openPopup(user)">{{ user.name }}</div>

          <div class="text-zinc-400 text-left w-[10%] text-left">{{ user.gender }}</div>
          <div class="text-left font-normal hover:font-semibold w-[10%] text-left">{{ user.country }}</div>


          <div class="text-gray text-zinc-400 w-[30%] text-right">{{ user.email }}</div>
        </div>
      </div>
    </div>
  </div>

  <div v-if="!isLoading"  class="flex justify-center my-10">
    <button @click="prevPage" :disabled="currentPage === 1"
      class="mx-2 bg-[#60B7D4] text-white font-bold py-2 px-4 rounded focus:outline-none">Previous</button>
    <button v-for="page in pageCount" :key="page" @click="goToPage(page)"
      :class="{ 'bg-[#60B7D4]': currentPage === page, 'text-white': currentPage === page, 'bg-gray-200': currentPage !== page }"
      class="mx-2 font-bold py-2 px-4 rounded focus:outline-none">{{ page }}</button>
    <button @click="nextPage" :disabled="currentPage === pageCount"
      class="mx-2 bg-[#60B7D4] text-white font-bold py-2 px-4 rounded focus:outline-none">Next</button>
  </div>

  <!-- Popup -->
  <div v-if="isPopupOpen" class="modal">
    <div class="modal-content">
      <span class="close" @click="closePopup">&times;</span>
      <h1 class="font-semibold text-2xl">{{ selectedUser.name }}</h1>
      <div class="user-info mt-5">
        <div class="info-item">
          <span class="label">Date:</span>
          <span class="value">{{ format_date(selectedUser.date) }}</span>
        </div>
        <div class="info-item">
          <span class="label">Age:</span>
          <span class="value">{{ selectedUser.age }}</span>
        </div>
        <div class="info-item">
          <span class="label">DOB:</span>
          <span class="value">{{ format_date(selectedUser.DOB) }}</span>
        </div>
        <div class="info-item">
          <span class="label">Gender:</span>
          <span class="value">{{ selectedUser.gender }}</span>
        </div>
        <div class="info-item">
          <span class="label">Country:</span>
          <span class="value">{{ selectedUser.country }}</span>
        </div>
        <div class="info-item">
          <span class="label">Email:</span>
          <span class="value">{{ selectedUser.email }}</span>
        </div>
      </div>

    </div>
  </div>

  <div v-if="!isLoading" class="flex justify-center my-10">
    <button @click="handleRefresh()"
      class="mx-4 bg-[#60B7D4] text-white font-bold py-2 px-4 rounded focus:outline-none">
      <span class="pi pi-refresh"></span>
      Refresh
    </button>
  </div>
</template>

<script>
import moment from 'moment'
import VuePaginate from 'vuejs-paginate';
export default {
  components: {
    VuePaginate,
  },
  data() {
    return {
      users: [],
      currentPage: 1,
      perPage: 2,
      searchQuery: '',
      items: [],
      selectedGender: '',
      isPopupOpen: false,
      selectedUser: null,
      isLoading: true,
    };
  },
  computed: {
    pageCount() {
      return Math.ceil(this.users.length / this.perPage);
    },
    paginatedUsers() {
      const start = (this.currentPage - 1) * this.perPage;
      const end = start + this.perPage;
      return this.users.slice(start, end);
    },



  },

  methods: {
    // API for Fetching User data
    async fetchUsers(pageNumber) {
      try {
        this.isLoading = true;
        const resultPerPage = 20;
        const response = await fetch(`https://randomuser.me/api/?page=${pageNumber}&results=${resultPerPage}`);
        const data = await response.json();
        this.users = data.results.map(result => ({
          date: result.registered.date,
          name: `${result.name.title} ${result.name.first} ${result.name.last}`,
          gender: result.gender,
          country: result.location.country,
          email: result.email,
          phone: result.phone,
          age: result.dob.age,
          DOB: result.dob.date
        }));
        this.isLoading = false;
      } catch (error) {
        console.error('Error fetching users:', error);
      }
    },
    // to format Date
    format_date(value) {
      if (value) {
        return moment(String(value)).format('ll')
      }
    },
    // to handle pagination next click
    nextPage() {
      if (this.currentPage < this.pageCount) {
        this.currentPage++;
      }
      this.fetchUsers(this.currentPage);

    },
    // to handle pagination previous click
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
      this.fetchUsers(this.currentPage);

    },
    goToPage(pageNumber) {
      this.currentPage = pageNumber;
    },
    // To open and Close Popup
    openPopup(user) {
      this.selectedUser = user;
      this.isPopupOpen = true;
    },
    closePopup() {
      this.selectedUser = null;
      this.isPopupOpen = false;
    },
    search() {
      console.log(this.searchQuery)

    },
    // to handle refresh Button Click
    handleRefresh() {
      this.fetchUsers(1);
      this.currentPage = 1;
    }
  },

  // this will call when Page load First Time
  mounted() {
    this.fetchUsers(1);
  }
};

</script>

<style scoped>
.card-table {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.card {
  width: 100%;
  padding: 20px;
  margin-bottom: 10px;
  border: 1px solid #ffffff;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.card h2 {
  font-size: 18px;
  margin-bottom: 10px;
}

.card p {
  margin: 5px 0;
}

.search-container {
  margin: 0 auto;
  padding: 20px;
  display: flex;
  justify-content: flex-end;
}

.filter-container {
  margin-bottom: 10px;
}

.filter-select {
  width: 100%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  outline: none;
}

.filter-select:focus {
  border-color: #007bff;
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
}

/* Modal styles */
.modal {
  display: block;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 40%;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
}

.close {
  color: #aaa;
  float: right;
  font-size: 24px;
  font-weight: bold;
  cursor: pointer;
}

.close:hover,
.close:focus {
  color: #000;
  text-decoration: none;
}

.user-info {
  margin-bottom: 20px;
}

.info-item {
  margin-bottom: 10px;
}

.label {
  font-size: 16px;
  color: #707070;
  margin-right: 10px;
}

.value {
  font-size: 16px;
}

.nk-vue {
  display: table
}

.nk-vue div {
  display: table-cell;
}

.loader {
  border: 10px solid #f3f3f3;
  border-radius: 50%;
  border-top: 10px solid #3498db;
  width: 80px;
  height: 80px;
  -webkit-animation: spin 2s linear infinite; /* Safari */
  animation: spin 2s linear infinite;
}

@-webkit-keyframes spin {
  0% { -webkit-transform: rotate(0deg); }
  100% { -webkit-transform: rotate(360deg); }
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>
