<script setup>
    import Cart from '@/components/CartIcon.vue'
    import Profile from '@/components/ProfileIcon.vue'
    import { RouterLink, RouterView } from 'vue-router'
    import { useAuthStore } from "@/stores/auth"
    import { storeToRefs } from 'pinia';

    const { isAuthenticated } = storeToRefs(useAuthStore())
</script>

<template>
    <!-- Medium and Bigger Screen Navbar -->
    <nav class="navbar navbar-expand-sm bg-light d-none d-md-block">
        <div class="container-fluid">
            <RouterLink class="navbar-brand" to="/">Ecommerce Test</RouterLink>
            <div class="input-group me-3">
                <input type="text" class="form-control" placeholder="Search Ecommerce" aria-label="Search" aria-describedby="button-addon2">
                <button @click="$router.push('/search')" role="link" class="btn btn-warning" type="button" id="button-addon2">Search</button>
            </div>
            <div class="navbar-nav">
                <RouterLink v-if="!isAuthenticated" class="nav-link" to="/login">Login</RouterLink>
                <RouterLink v-if="!isAuthenticated" class="nav-link" to="/signup">Register</RouterLink>
                <RouterLink v-if="isAuthenticated" class="nav-link" to="/profile"><Profile /></RouterLink>
                <RouterLink v-if="isAuthenticated" class="nav-link" to="/cart"><Cart /></RouterLink>
            </div>
        </div>
    </nav>

    <!-- Small Screen Navbar -->
    <nav class="navbar navbar-expand-sm bg-light d-block d-md-none">
        <div class="container-fluid">
            <div class="input-group">
                <input type="text" class="form-control" placeholder="Search Ecommerce" aria-label="Search" aria-describedby="button-addon2">
                <button class="btn btn-warning" type="button" id="button-addon2">Search</button>
            </div>
        </div>
    </nav>

    <RouterView />
</template>
