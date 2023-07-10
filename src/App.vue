<script setup lang="ts">
// @ts-nocheck
import Rating from "./components/Rating.vue";
import {ref} from "vue";

const val1 = ref(0);
const val2 = ref(0);
const val3 = ref(0);
const val4 = ref(0);

</script>

<template>
    <div id="root">
        <div>
            <h2>Responsive</h2>
            <rating @click="(v) => val1 = v"></rating>
        </div>
        <div>
            <h2>Code</h2>
            <rating @click="(v) => val2 = v"></rating>
        </div>
        <div>
            <h2>Intégration</h2>
            <rating @click="(v) => val3 = v"></rating>
        </div>
        <div>
            <h2>Vertu</h2>
            <rating @click="(v) => val4 = v"></rating>
        </div>
        <transition>
            <div v-if="val1 != 0 && val2 != 0 && val3 != 0 && val4 != 0">
                <h2>Total</h2>
                <rating :value="(val1+val2+val3+val4)/4"></rating>
            </div>
        </transition>
    </div>
</template>

<style scoped>
    #root {
        display: grid;
        grid-column: 2;
        grid-row: 2;
    }

    .v-enter-active,
    .v-leave-active {
        transition: all 0.5s ease;
    }

    .v-enter-from,
    .v-leave-to {
        transform: translateX(-80px);
        opacity: 0;
    }
</style>
