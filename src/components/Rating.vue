<template>
    <div @mouseover="hovered = true" @mouseleave="hovered = false">
      <Star :key="index" v-if="hovered" @hover-in="(r) => hoveredStar(r, index)" @hover-move="(r) => hoveredStar(r, index)" @hover-out="stopHovering" @click="(e) => clickOnStar(e, index)" :size="size" :rate="value" ref="starsRefs" v-for="(value, index) in starRates"></Star>
      <Star v-else :size="size" :rate="value" ref="starsRefs" v-for="value in starRatesSaved"></Star>
    </div>
</template>

<script setup lang="ts">

import {ref} from "vue";
import Star from "./Star.vue";

const starsRefs = ref([]);

const starRates = ref([0,0,0,0,0]);
const starRatesSaved = ref([0,0,0,0,0]);
const hovered = ref(false);

const props = defineProps({
    size: Number,
});
function hoveredStar(rate, id: number) {
    starRates.value = [0,0,0,0,0];
    for (let i = 0; i < id; i++) {
        starRates.value[i] = 4;
    }
    starRates.value[id] = rate;
}

function clickOnStar(event, id: number) {
    starRatesSaved.value = starRates.value;
}

function stopHovering() {
    starRates.value = [0,0,0,0,0];
}
</script>

<style scoped>

</style>