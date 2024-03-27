<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import Header from './components/Header.vue'
import UserProfile from './components/Profile.vue'
import Table from './components/Table.vue'


const user = {
    name: 'John Doe',
    image: '../public/Avatar.png',
    lastOnline: 'Last online: 1 hour ago'
}

const userData = {
    name: 'John Doe',
    description: 'Some description',
}

</script>

<template>

    <Header />
    <UserProfile :user="user" />
    <Table :user="userData" />


</template>
