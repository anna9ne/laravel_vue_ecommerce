<template>
    <div class="min-h-full flex">
        <!-- sidebar -->
        <Sidebar :class="{'-ml-[200px]' : !sidebarOpened}" />
        <!-- /sidebar -->

        <div class="flex-1">
            <Navbar @toggle-sidebar="toggleSidebar"></Navbar>

            <!-- content -->
            <main class="p-6">
                <router-view></router-view>
            </main>
            <!-- /content -->
        </div>
    </div>
</template>

<script setup>
    import {ref, onMounted, onUnmounted} from 'vue'
    import Sidebar from "./Sidebar.vue";
    import Navbar from "./Navbar.vue";
    import store from "../store/index.js";

    const {title} = defineProps({
        title: String,
    })

    const sidebarOpened = ref(true)

    function toggleSidebar() {
       sidebarOpened.value = !sidebarOpened.value
    }

    onMounted(() => {
        store.dispatch('getUser')
        handleSidebarOpened()
        window.addEventListener('resize', handleSidebarOpened)
    })

    onUnmounted(() => {
        window.removeEventListener('resize', handleSidebarOpened)
    })

    function handleSidebarOpened() {
        if (window.outerWidth <= 768) {
            sidebarOpened.value = false
        } else {
            sidebarOpened.value = true
        }
    }


</script>

<style scoped>

</style>
