<script setup>
import { ref, onMounted } from 'vue';
import Panel from 'primevue/panel';
import Fieldset from 'primevue/fieldset';
import Reference from './Reference.vue';

const charging = ref('N/A');
const level = ref('N/A');
const chargingTime = ref('N/A');
const dischargingTime = ref('N/A');

const batteryInfo = [
    {
        name: 'Charging',
        value: charging
    },
    {
        name: 'Level',
        value: level
    },
    {
        name: 'Charging Time',
        value: chargingTime
    },
    {
        name: 'Discharging Time',
        value: dischargingTime
    },
];

onMounted(() => {
    update();
})

function update() {
    try {
        navigator.getBattery().then((battery) => {
            function updateAllBatteryInfo() {
                updateChargeInfo();
                updateLevelInfo();
                updateChargingInfo();
                updateDischargingInfo();
            }
            updateAllBatteryInfo();

            battery.addEventListener("chargingchange", () => {
                updateChargeInfo();
            });
            function updateChargeInfo() {
                charging.value = battery.charging;
            }

            battery.addEventListener("levelchange", () => {
                updateLevelInfo();
            });
            function updateLevelInfo() {
                level.value = `${battery.level * 100}%`;
            }

            battery.addEventListener("chargingtimechange", () => {
                updateChargingInfo();
            });
            function updateChargingInfo() {
                chargingTime.value = battery.chargingTime == Infinity ? 'N/A' : battery.chargingTime;
            }

            battery.addEventListener("dischargingtimechange", () => {
                updateDischargingInfo();
            });
            function updateDischargingInfo() {
                dischargingTime.value = battery.dischargingTime == Infinity ? 'N/A' : battery.dischargingTime;
            }
        });
    } catch (err) {
        console.log(err);
    }
}

</script>

<template>
    <Panel class="m-4" toggleable collapsed>
        <template #header>
            <span class="m-4 text-2xl font-bold">Battery Status API</span>
        </template>
        <div class="grid justify-items-center">
            <div>
                <div v-for="info in batteryInfo" class="inline-block w-1/2">
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
                <Reference url="https://developer.mozilla.org/en-US/docs/Web/API/Battery_Status_API" />
            </div>
        </template>
    </panel>
</template>

<style scoped></style>