<script setup>
import { ref } from 'vue';
import Panel from 'primevue/panel';
import Message from 'primevue/message';
import Button from 'primevue/button';
import { useToast } from 'primevue/usetoast';
import Reference from './Reference.vue';

const toast = useToast();

const error = ref('');
const nfcRead = ref(false);
const nfcReadError = ref(false);
const scanning = ref(false);
var ndef = null;
var scanController = null;

function setupNDEFReader() {
    try {
        ndef = new NDEFReader();
        ndef.addEventListener('reading', () => {
            nfcRead.value = true;
            nfcReadError.value = false;

            toast.add({ severity: 'success', summary: 'Success', detail: `Read an NFC`, life: 5000 });
        });
        ndef.addEventListener('readingerror', () => {
            nfcReadError.value = true;

            toast.add({ severity: 'error', summary: 'Failed', detail: `Failed to read NFC`, life: 5000 });
        });
    }
    catch (err) {
        error.value = err;
    }
}

async function scanNFC() {
    try {
        scanController = new AbortController();
        await ndef.scan({ signal: scanController.signal });
        scanning.value = true;
    }
    catch (err) {
        error.value = err;
    }
}

function stopScanNFC() {
    scanController.abort();
    scanning.value = false;
}

setupNDEFReader();
</script>

<template>
    <Panel class="m-4" toggleable collapsed>
        <template #header>
            <span class="m-4 text-2xl font-bold">
                Web NFC API
            </span>
        </template>
        <Message v-if="error" class="m-2" severity="error">{{ error }}</Message>
        <div class="grid justify-items-center">
            <Message v-if="!nfcRead" class="m-2" severity="info">Move the device close to an NFC tag (or tag to device)
            </Message>

            <Button v-if="!scanning" class="m-2" label="Scan" @click="scanNFC()"></Button>
            <Button v-if="scanning" class="m-2" label="Stop" @click="stopScanNFC()" severity="danger"></Button>
        </div>
        <template #footer>
            <div class="flex justify-end">
                <Reference url="https://developer.mozilla.org/en-US/docs/Web/API/Web_NFC_API" />
            </div>
        </template>
    </Panel>
</template>
