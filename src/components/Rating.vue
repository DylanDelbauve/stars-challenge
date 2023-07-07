<template>
    <div @mouseover="hovered = true" @mouseleave="hovered = false">
      <Star :key="index" v-if="hovered" @in="(r: number) => hoveredStar(r, index)" @out="stopHovering" @click="clickOnStar" :size="size" :rate="value" v-for="(value, index) in starRates"></Star>
      <Star v-else :size="size" :rate="value" ref="starsRefs" v-for="value in starRatesSaved"></Star>
    </div>
</template>

<script setup lang="ts">
import {Ref, ref} from "vue";
import Star from "./Star.vue";

defineProps({
    size: Number,
});

const starRates: Ref<Array<number>>  = ref([0,0,0,0,0]);
const starRatesSaved: Ref<Array<number>> = ref([0,0,0,0,0]);
const hovered: Ref<boolean> = ref(false);


function hoveredStar(index: number, rate: number): void {
    starRates.value = [0,0,0,0,0];
    for (let i = 0; i < index; i++) {
        starRates.value[i] = 4;
    }
    starRates.value[index] = rate;
}

function clickOnStar(): void {
    starRatesSaved.value = starRates.value;
}

function stopHovering(): void {
    starRates.value = [0,0,0,0,0];
}


</script>

<style scoped>

</style>