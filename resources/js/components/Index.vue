<template>
    <div>
        <router-link :to="{ name: 'fruit.index' }">List</router-link>
        <router-link v-if="!access_token" :to="{ name: 'user.login' }">Login</router-link>
        <router-link v-if="!access_token" :to="{ name: 'user.registration' }">Registration</router-link>
        <router-link v-if="access_token" :to="{ name: 'user.personal' }">Personal</router-link>
        <a v-if="access_token" @click.prevent="logout" href="#">Logout</a>
        <router-view></router-view>
    </div>
</template>

<script>
import API from "../api"
export default {
    name: "Index",

    data() {
        return {
            access_token: null
        }
    },

    mounted() {
        this.getAccessToken()
    },

    updated() {
        this.getAccessToken()
    },

    methods: {
        getAccessToken() {
            this.access_token = localStorage.getItem('access_token')
        },

        logout() {
            API.post('/api/auth/logout')
                .then( res => {
                    localStorage.removeItem('access_token')
                    this.$router.push({name: "user.login"})
                })
        }
    }
}
</script>

<style scoped>

</style>