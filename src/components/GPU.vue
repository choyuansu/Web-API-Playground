<script setup>
import { ref, onMounted } from 'vue';
import Panel from 'primevue/panel';
import Button from 'primevue/button';
import Message from 'primevue/message';
import Reference from './Reference.vue';

const error = ref('');
var loaded = false;

function loadScripts() {
    if (loaded) return;

    const mainjs = document.createElement('script');
    const iframeHelper = document.createElement('script');

    mainjs.setAttribute('src', '/webgpu/cameras/main.js');
    mainjs.setAttribute('type', 'module');

    iframeHelper.setAttribute('src', '/webgpu/cameras/js/iframe-helper.js');
    iframeHelper.setAttribute('type', 'module');

    document.head.appendChild(mainjs);
    document.head.appendChild(iframeHelper);

    loaded = true;
}
</script>

<template>
    <Panel class="m-4" toggleable @toggle="loadScripts()" collapsed>
        <template #header>
            <span class="m-4 text-2xl font-bold">
                Web GPU API
            </span>
        </template>
        <Message v-if="error" class="m-2" severity="error">{{ error }}</Message>
        <div class="grid justify-items-center">
            <canvas class="w-full md:max-w-3/4 aspect-square"></canvas>
        </div>
        <template #footer>
            <div class="flex justify-between">
                <Button as="a" label="Source" href="https://webgpu.github.io/webgpu-samples/?sample=cameras"
                    target="_blank" rel="noopener" link></Button>
                <Reference url="https://developer.mozilla.org/en-US/docs/Web/API/WebGPU_API" />
            </div>
        </template>
    </Panel>
</template>
