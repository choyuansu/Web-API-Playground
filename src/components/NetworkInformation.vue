<script setup>
import { ref } from 'vue';
import Panel from 'primevue/panel';
import Fieldset from 'primevue/fieldset';
import Message from 'primevue/message';
import Reference from './Reference.vue';

const error = ref('');

const downlink = ref(0);
const downlinkMax = ref(0);
const effectiveType = ref('');
const rtt = ref(0);
const saveData = ref(false);
const type = ref('');


const information = [
    {
        name: 'Downlink',
        value: downlink
    },
    {
        name: 'Downlink Max',
        value: downlinkMax
    },
    {
        name: 'Effective Type',
        value: effectiveType
    },
    {
        name: 'Round-trip Time',
        value: rtt
    },
    {
        name: 'Save Data',
        value: saveData
    },
    {
        name: 'Type',
        value: type
    },
];

function updateConnectionStatus() {
    try {
        const connection = navigator.connection;
        navigator.connection.addEventListener("change", updateConnectionStatus);

        downlink.value = connection.downlink;
        downlinkMax.value = connection.downlinkMax;
        effectiveType.value = connection.effectiveType;
        rtt.value = connection.rtt;
        saveData.value = connection.saveData;
        type.value = connection.type;

        error.value = '';
    }
    catch (err) {
        error.value = err;
    }
}

updateConnectionStatus();
</script>

<template>
    <Panel class="m-4" toggleable collapsed>
        <template #header>
            <span class="m-4 text-2xl font-bold">
                Network Information API
            </span>
        </template>
        <Message v-if="error" severity="error">{{ error }}</Message>
        <div class="grid justify-items-center">
            <div>
                <div v-for="info in information" class="inline-block w-1/2">
                    <div class="m-2">
                        <Fieldset :legend="info.name">
                            {{ info.value.value }}
                        </Fieldset>
                    </div>
                </div>
            </div>
        </div>
        <template #footer>
            <div class="flex justify-end">
                <Reference url="https://developer.mozilla.org/en-US/docs/Web/API/Network_Information_API" />
            </div>
        </template>
    </Panel>
</template>

<style scoped></style>