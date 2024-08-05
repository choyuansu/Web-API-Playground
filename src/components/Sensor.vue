<script setup>
import { ref } from 'vue';
import Panel from 'primevue/panel';
import Fieldset from 'primevue/fieldset';
import Message from 'primevue/message';
import Reference from './Reference.vue';

const error = ref('');

const sensors = {
    accelerometer: {
        name: 'Accelerometer',
        noAccess: ref(true),
        props: {
            x: ref(0),
            y: ref(0),
            z: ref(0),
        }
    },
    absoluteOrientationSensor: {
        name: 'Absolute Orientation Sensor',
        noAccess: ref(true),
        props: {
            quaternion: ref([0, 0, 0, 0])
        }
    },
    ambientLightSensor: {
        name: 'Ambient Light Sensor',
        noAccess: ref(true),
        props: {
            illuminance: ref(0)
        }
    },
    gravitySensor: {
        name: 'Gravity Sensor',
        noAccess: ref(true),
        props: {
            x: ref(0),
            y: ref(0),
            z: ref(0),
        }
    },
    gyroscope: {
        name: 'Gyroscope',
        noAccess: ref(true),
        props: {
            x: ref(0),
            y: ref(0),
            z: ref(0),
        }
    },
    linearAccelerationSensor: {
        name: 'Linear Acceleration Sensor',
        noAccess: ref(true),
        props: {
            x: ref(0),
            y: ref(0),
            z: ref(0),
        }
    },
    magnetometer: {
        name: 'Magnetometer',
        noAccess: ref(true),
        props: {
            x: ref(0),
            y: ref(0),
            z: ref(0),
        }
    },
    relativeOrientationSensor: {
        name: 'Relative Orientation Sensor',
        noAccess: ref(true),
        props: {
            quaternion: ref([0, 0, 0, 0])
        }
    },
};

function setupSensors() {
    // setup Accelerometer
    try {
        const accelerometer = new Accelerometer({ referenceFrame: "device" });
        accelerometer.addEventListener("reading", () => {
            sensors.accelerometer.props.x.value = Math.round(accelerometer.x * 10) / 10;
            sensors.accelerometer.props.y.value = Math.round(accelerometer.y * 10) / 10;
            sensors.accelerometer.props.z.value = Math.round(accelerometer.z * 10) / 10;
        });
        accelerometer.start();
        sensors.accelerometer.noAccess.value = false;
    } catch (err) {
        if (err.name !== 'ReferenceError') {
            error.value = err;
        }
    }

    // setup AbsoluteOrientationSensor
    try {
        const absoluteOrientationSensor = new AbsoluteOrientationSensor({ frequency: 60, referenceFrame: "device" });
        absoluteOrientationSensor.addEventListener("reading", () => {
            sensors.absoluteOrientationSensor.props.quaternion.value = absoluteOrientationSensor.quaternion.map((e) => {
                return Math.round(e * 10) / 10;
            });
        });
        absoluteOrientationSensor.start();
        sensors.absoluteOrientationSensor.noAccess.value = false;
    } catch (err) {
        if (err.name !== 'ReferenceError') {
            error.value = err;
        }
    }

    // Not supported
    // setup AmbientLightSensor
    // try {
    //     const ambientLightSensor = new AmbientLightSensor({ frequency: 10 });
    //     ambientLightSensor.addEventListener("reading", () => {
    //         sensors.ambientLightSensor.props.illuminance.value = ambientLightSensor.illuminance;
    //         console.log(ambientLightSensor.illuminance);
    //     });
    //     ambientLightSensor.start();
    //     sensors.ambientLightSensor.noAccess.value = false;
    // } catch (err) {
    //     if (err.name !== 'ReferenceError') {
    //         error.value = err;
    //     }
    // }

    // setup Gravity Sensor
    try {
        const gravitySensor = new GravitySensor({ frequency: 60 });
        gravitySensor.addEventListener("reading", () => {
            sensors.gravitySensor.props.x.value = Math.round(gravitySensor.x * 10) / 10;
            sensors.gravitySensor.props.y.value = Math.round(gravitySensor.y * 10) / 10;
            sensors.gravitySensor.props.z.value = Math.round(gravitySensor.z * 10) / 10;
        });
        gravitySensor.start();
        sensors.gravitySensor.noAccess.value = false;
    } catch (err) {
        if (err.name !== 'ReferenceError') {
            error.value = err;
        }
    }

    // setup Gyroscope
    try {
        const gyroscope = new Gyroscope({ frequency: 60 });
        gyroscope.addEventListener("reading", () => {
            sensors.gyroscope.props.x.value = Math.round(gyroscope.x * 10) / 10;
            sensors.gyroscope.props.y.value = Math.round(gyroscope.y * 10) / 10;
            sensors.gyroscope.props.z.value = Math.round(gyroscope.z * 10) / 10;
        });
        gyroscope.start();
        sensors.gyroscope.noAccess.value = false;
    } catch (err) {
        if (err.name !== 'ReferenceError') {
            error.value = err;
        }
    }

    // setup Linear Acceleration Sensor
    try {
        const linearAccelerationSensor = new LinearAccelerationSensor({ frequency: 60 });
        linearAccelerationSensor.addEventListener("reading", () => {
            sensors.linearAccelerationSensor.props.x.value = Math.round(linearAccelerationSensor.x * 10) / 10;
            sensors.linearAccelerationSensor.props.y.value = Math.round(linearAccelerationSensor.y * 10) / 10;
            sensors.linearAccelerationSensor.props.z.value = Math.round(linearAccelerationSensor.z * 10) / 10;
        });
        linearAccelerationSensor.start();
        sensors.linearAccelerationSensor.noAccess.value = false;
    } catch (err) {
        if (err.name !== 'ReferenceError') {
            error.value = err;
        }
    }

    // Not supported
    // setup Magnetometer
    // try {
    //     const magnetometer = new Magnetometer({ frequency: 60 });
    //     magnetometer.addEventListener("reading", () => {
    //         sensors.magnetometer.props.x.value = Math.round(magnetometer.x * 10) / 10;
    //         sensors.magnetometer.props.y.value = Math.round(magnetometer.y * 10) / 10;
    //         sensors.magnetometer.props.z.value = Math.round(magnetometer.z * 10) / 10;
    //     });
    //     magnetometer.start();
    //     sensors.magnetometer.noAccess.value = false;
    // } catch (err) {
    //     if (err.name !== 'ReferenceError') {
    //         error.value = err;
    //     }
    // }

    // setup RelativeOrientationSensor
    try {
        const relativeOrientationSensor = new RelativeOrientationSensor({ frequency: 60, referenceFrame: "device" });
        relativeOrientationSensor.addEventListener("reading", () => {
            sensors.relativeOrientationSensor.props.quaternion.value = relativeOrientationSensor.quaternion.map((e) => {
                return Math.round(e * 10) / 10;
            });
        });
        relativeOrientationSensor.start();
        sensors.relativeOrientationSensor.noAccess.value = false;
    } catch (err) {
        if (err.name !== 'ReferenceError') {
            error.value = err;
        }
    }

};
setupSensors();
</script>

<template>
    <Panel class="m-4" toggleable collapsed>
        <template #header>
            <span class="m-4 text-2xl font-bold">
                Sensor APIs
            </span>
        </template>
        <Message v-if="error" class="m-2" severity="error">{{ error }}</Message>
        <div class="grid justify-items-center">
            <div>
                <div v-for="sensor in sensors">
                    <div v-if="!sensor.noAccess.value" class="m-2">
                        <Fieldset :legend="sensor.name">
                            <div v-for="(propValue, propName) in sensor.props" class="inline-block m-2">
                                <Fieldset :legend="propName">{{ propValue.value }}</Fieldset>
                            </div>
                        </Fieldset>
                    </div>
                </div>
            </div>
        </div>
        <template #footer>
            <div class="flex justify-end">
                <Reference url="https://developer.mozilla.org/en-US/docs/Web/API/Sensor_APIs" />
            </div>
        </template>
    </Panel>
</template>