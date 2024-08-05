<script setup>
import { ref } from 'vue';
import Panel from 'primevue/panel';
import Button from 'primevue/button';
import Message from 'primevue/message';
import Image from 'primevue/image';
import Reference from './Reference.vue';

const error = ref('');

let cameras = ref([]);
let imageCapture = null;
const cameraView = ref(null);
const photoUrl = ref('');
const showCameraView = ref(false);
const showPhotoView = ref(false);


navigator.mediaDevices.enumerateDevices()
    .then((devices) => {
        devices.forEach((device) => {
            if (device.kind == 'videoinput') {
                cameras.value.push({ label: device.label, id: device.deviceId });
            }
        })
        cameras.value.sort((a, b) => {
            if (a.label < b.label) return -1;
            else return 1;
        });
        error.value = '';
    })
    .catch((err) => {
        error.value = err;
    });

async function capture(id) {
    try {
        showCameraView.value = true;
        const mediaStream = await navigator.mediaDevices.getUserMedia({ video: { deviceId: { exact: id } } })
        cameraView.value.srcObject = mediaStream;
        imageCapture = new ImageCapture(mediaStream.getVideoTracks()[0]);

        error.value = '';
    }
    catch (err) {
        error.value = err;
        showCameraView.value = false;
    }
}

async function takePhoto() {
    try {
        showPhotoView.value = true;
        const imageBlob = await imageCapture.takePhoto();
        photoUrl.value = URL.createObjectURL(imageBlob);

        error.value = '';
    }
    catch (err) {
        error.value = err;
        showPhotoView.value = false;
    }
}
</script>

<template>
    <Panel class="m-4" toggleable collapsed>
        <template #header>
            <span class="m-4 text-2xl font-bold">
                Media Capture and Streams API
            </span>
        </template>
        <Message v-if="error" class="m-2" severity="error">{{ error }}</Message>
        <div class="grid justify-items-center">
            <div class="grid gap-4 grid-cols-2 place-content-center">
                <Button v-for="camera in cameras" :label="camera.label" :id="camera.id"
                    @click="capture(camera.id)"></Button>
            </div>
            <video v-if="showCameraView" ref="cameraView" class="m-2" autoplay></video>
            <Button v-if="showCameraView" class="m-2" label="Take Photo" @click="takePhoto()"></Button>
            <Image v-if="showPhotoView" :src="photoUrl" class="m-2" />
        </div>
        <template #footer>
            <div class="flex justify-end">
                <Reference url="https://developer.mozilla.org/en-US/docs/Web/API/Media_Capture_and_Streams_API" />
            </div>
        </template>
    </Panel>
</template>

<style scoped></style>