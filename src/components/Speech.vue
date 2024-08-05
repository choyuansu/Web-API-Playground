<script setup>
import { ref, defineModel } from 'vue';
import Panel from 'primevue/panel';
import Message from 'primevue/message';
import Button from 'primevue/button';
import Fieldset from 'primevue/fieldset';
import Textarea from 'primevue/textarea';
import Accordion from 'primevue/accordion';
import AccordionPanel from 'primevue/accordionpanel';
import AccordionHeader from 'primevue/accordionheader';
import AccordionContent from 'primevue/accordioncontent';
import { useToast } from 'primevue/usetoast';
import Reference from './Reference.vue';

const activePanel = defineModel('activePanel');
const recognitionError = ref('');
const toast = useToast();

// recognition
const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
var recognition = null;
const srStarted = ref(false)
const result = ref('');

// synthesis
const toUtter = defineModel('toUtter');
const synthesisError = ref('');

function setupSpeechRecognition() {
    try {
        recognition = new SpeechRecognition();

        recognition.onspeechend = () => {
            recognition.stop();
            srStarted.value = false;
        };

        recognition.onresult = (event) => {
            result.value = event.results[0][0].transcript;
        };

        recognition.onnomatch = () => {
            toast.add({ severity: 'error', summary: 'Failed', detail: `Failed to recognize speech`, life: 5000 });
            srStarted = false;
        };
    }
    catch (err) {
        recognitionError.value = err;
    }
}

function startSpeechRecognition() {
    recognition.start();
    srStarted.value = true;
}

function abortSpeechRecognition() {
    recognition.abort();
    srStarted.value = false;
}

function copyTranscriptToUtter() {
    toUtter.value = result.value;
    activePanel.value = '1';
}

function startSpeechSynthesis() {
    try {
        let utterance = new SpeechSynthesisUtterance(toUtter.value);
        window.speechSynthesis.speak(utterance);
        synthesisError.value = '';
    }
    catch (err) {
        synthesisError.value = err;
    }
}

setupSpeechRecognition();
</script>

<template>
    <Panel class="m-4" toggleable collapsed>
        <template #header>
            <span class="m-4 text-2xl font-bold">
                Web Speech API
            </span>
        </template>
        <Accordion v-model:value="activePanel">
            <AccordionPanel value="0">
                <AccordionHeader>Speech Recognition</AccordionHeader>
                <AccordionContent>
                    <Message v-if="recognitionError" class="m-2" severity="error">{{ recognitionError }}</Message>
                    <div class="grid justify-items-center m-2">
                        <Button v-if="!srStarted" label="Start" @click="startSpeechRecognition()"></Button>
                        <Button v-if="srStarted" label="Abort" severity="danger"
                            @click="abortSpeechRecognition()"></Button>
                        <Fieldset v-if="result" legend="Transcript" class="w-full m-2">
                            <p>{{ result }}</p>
                        </Fieldset>
                        <Button v-if="result" class="m-2" label="Synthesize" severity="secondary"
                            @click="copyTranscriptToUtter()"></Button>
                    </div>
                </AccordionContent>
            </AccordionPanel>
            <AccordionPanel value="1">
                <AccordionHeader>Speech Synthesis</AccordionHeader>
                <AccordionContent>
                    <Message v-if="synthesisError" class="m-2" severity="error">{{ synthesisError }}</Message>
                    <div class="grid justify-items-center m-2">
                        <Textarea v-model="toUtter" rows="3" class="m-2 w-full md:w-1/2" autoResize></Textarea>
                        <Button class="m-2" label="Start" @click="startSpeechSynthesis()"></Button>
                    </div>
                </AccordionContent>
            </AccordionPanel>
        </Accordion>
        <template #footer>
            <div class="flex justify-end">
                <Reference url="https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API" />
            </div>
        </template>
    </Panel>
</template>