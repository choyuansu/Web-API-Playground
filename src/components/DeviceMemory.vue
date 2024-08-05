<script setup>
import { ref } from 'vue';
import Panel from 'primevue/panel';
import Message from 'primevue/message';
import Reference from './Reference.vue';

const error = ref('');
const memory = ref('N/A');

function setup() {
    try {
        memory.value = navigator.deviceMemory;
    } catch (err) {
        error.value = err;
    }
}

setup()
</script>

<template>
    <Panel class="m-4" toggleable collapsed>
        <template #header>
            <span class="m-4 text-2xl font-bold">
                Device Memory API
            </span>
        </template>
        <Message v-if="error" class="m-2" severity="error">{{ error }}</Message>
        <div class="grid justify-items-center">
            <p>This device has at least {{ memory }} GiB of RAM.</p>
        </div>
        <template #footer>
            <div class="flex justify-end">
                <Reference url="https://developer.mozilla.org/en-US/docs/Web/API/Device_Memory_API" />
            </div>
        </template>
    </Panel>
</template>
