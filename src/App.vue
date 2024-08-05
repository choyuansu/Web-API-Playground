<script setup>
import Toast from 'primevue/toast';
import Battery from './components/Battery.vue';
import DeviceMemory from './components/DeviceMemory.vue';
import Geolocation from './components/Geolocation.vue';
import MediaStream from './components/MediaStream.vue';
import NetworkInformation from './components/NetworkInformation.vue';
import Sensor from './components/Sensor.vue';
import Vibration from './components/Vibration.vue';
import NFC from './components/NFC.vue';
import GPU from './components/GPU.vue';
import Speech from './components/Speech.vue';
</script>

<template>
    <Toast />

    <h1 class="text-4xl font-bold text-center m-8">Web API Playground</h1>
    <Battery />
<DeviceMemory />
    <Geolocation />
    <MediaStream />
    <NetworkInformation />
    <Sensor />
    <Vibration />
    <NFC />
    <GPU />
    <Speech />
</template>

<style scoped></style>
