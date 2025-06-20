<template>
    <div v-if="show" class="md:hidden py-4 border-t">
        <nav class="flex flex-col space-y-2">
            <a href="#" class="text-gray-700 hover:text-purple-600 py-2">Home</a>
            <a href="#" class="text-gray-700 hover:text-purple-600 py-2">Men</a>
            <a href="#" class="text-gray-700 hover:text-purple-600 py-2">Women</a>
            <a href="#" class="text-gray-700 hover:text-purple-600 py-2">Brands</a>
            <a href="#" class="text-gray-700 hover:text-purple-600 py-2">About</a>
        </nav>
    </div>
</template>
<script setup>
const props = defineProps({
    show: Boolean,
})
</script>