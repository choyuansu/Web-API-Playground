<script setup>
import Button from 'primevue/button';

defineProps({
    url: String
});
</script>

<template>
    <Button as="a" label="API Reference" :href="url" target="_blank" rel="noopener" link></Button>
</template>

<style scoped></style>