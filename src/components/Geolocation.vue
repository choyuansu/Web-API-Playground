<script setup>
import { ref } from 'vue';
import Panel from 'primevue/panel';
import Message from 'primevue/message';
import Button from 'primevue/button';
import Fieldset from 'primevue/fieldset';
import Reference from './Reference.vue';

const error = ref('');
const coords = ref('N/A');
const mapUrl = ref('');

function getGeolocation() {
    try {
        if ("geolocation" in navigator) {
            navigator.geolocation.getCurrentPosition((position) => {
                coords.value = position.coords.latitude + ', ' + position.coords.longitude;
                mapUrl.value = `https://www.openstreetmap.org/#map=18/${position.coords.latitude}/${position.coords.longitude}`;
                navigator.geolocation.watchPosition((position) => {
                    coords.value = position.coords.latitude + ', ' + position.coords.longitude;
                    mapUrl.value = `https://www.openstreetmap.org/#map=18/${position.coords.latitude}/${position.coords.longitude}`;
                });
            });
            error.value = '';
        } else {
            error.value = 'Geolocation unavailable';
        }
    } catch (err) {
        error.value = err;
        coords.value = 'N/A';
    }
}
</script>

<template>
    <Panel class="m-4" toggleable collapsed>
        <template #header>
            <span class="m-4 text-2xl font-bold">
                Geolocation API
            </span>
        </template>
        <Message v-if="error" class="m-2" severity="error">{{ error }}</Message>
        <div class="grid justify-items-center">
            <Button v-if="coords == 'N/A'" label="Get geolocation" @click="getGeolocation()"></Button>
            <Fieldset legend="Coordinates">
                <span v-if="coords == 'N/A'">{{ coords }}</span>
                <Button v-if="coords != 'N/A'" as="a" :label="coords" :href="mapUrl" target="_blank" rel="noopener"
                    link></Button>
            </Fieldset>
        </div>
        <template #footer>
            <div class="flex justify-end">
                <Reference url="https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API" />
            </div>
        </template>
    </Panel>
</template>
