<template>
    <div
        class="bg-gray-800 text-white rounded-full px-4 py-2 w-28 text-center cursor-pointer"
        :class="{'border-2 border-red-200 border-fade-in': active}"
        @click="$emit('select', value)"
    >
        {{ label }}
    </div>
</template>

<script setup>
defineProps({
    label: String,
    value: String,
    active: Boolean
});
</script>