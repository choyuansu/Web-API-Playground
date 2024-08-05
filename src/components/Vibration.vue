<script setup>
import { ref } from 'vue';
import Panel from 'primevue/panel';
import Button from 'primevue/button';
import Message from 'primevue/message';
import Reference from './Reference.vue';

const error = ref('');

function vibrate() {
    const vibrated = navigator.vibrate(200);
    if (!vibrated) {
        error.value = 'Vibration not supported';
    }
    error.value = '';
}

</script>

<template>
    <Panel class="m-4" toggleable collapsed>
        <template #header>
            <span class="m-4 text-2xl font-bold">
                Vibration API
            </span>
        </template>
        <Message v-if="error" class="m-2" severity="error">{{ error }}</Message>
        <div class="grid justify-items-center">
            <Button label="Vibrate" @click="vibrate()"></Button>
        </div>
        <template #footer>
            <div class="flex justify-end">
                <Reference url="https://developer.mozilla.org/en-US/docs/Web/API/Vibration_API" />
            </div>
        </template>
    </Panel>
</template>
