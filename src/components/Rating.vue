<template>
    <div>
        <div class="stars-result" ref="result" v-if="value != null && value != 0">
            <svg :key="n" v-for="n in 5"
                 width="64" height="64" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
                 viewBox="0 0 22 20">
                <path d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z"/>
            </svg>
        </div>
        <div class="stars">
            <svg @mousemove="hover(index)" @mouseout="out" @click.prevent="click(index)" class="star" :class="{clickAnimation: onClickAnimation}" ref="stars" v-for="index in 5"
                 width="64" height="64" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
                 viewBox="0 0 22 20">
                <path d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z"/>
            </svg>
        </div>

    </div>
</template>

<script setup lang="ts">
// @ts-nocheck
const props = defineProps({
    disabled: {
        type: Boolean,
        default: false
    },
    value: {
        type: Number
    }
})

const emit = defineEmits(['click'])

import {onMounted, Ref, ref, watch} from "vue";

const stars = ref([]);

const result: Ref<HTMLElement> = ref(document);

const rate: Ref<Number> = ref<Number>(0);

const onClickAnimation = ref(false);

onMounted(() => {
    watch(() => props.value, (newValue) => {
        if(newValue != null && newValue != 0 && result.value != null) {
            result.value.style.maskImage = 'linear-gradient(90deg, #000000 0%, #000000 ' + newValue*20 + '%, transparent ' + newValue*20 + '%, transparent 100%)'
            result.value.style.fill = `hsl(${newValue * 20}, 100%, 65%)`;
        }
    })
})
function hover(index: Number) {
    for (let i = 0; i < 5; i++) {
        stars.value[i].classList.remove('active', 'near');
        stars.value[i].style.fill = "";
    }
    for (let i = 0; i < index; i++) {
        let star = stars.value[i];
        star.classList.add('active');
        star.style.fill = `hsl(${index * 20}, 100%, 65%)`;
    }
    if (index != 1)
        stars.value[index - 2].classList.add('near');
}

function click(index: number) {
    rate.value = index;
    let delay = 20;
    stars.value.forEach((star, i) => {
        star.style.animationDelay = `${delay * i}ms`;
    });
    onClickAnimation.value = true
    setTimeout(() => {
        onClickAnimation.value = false
    }, 2000);

    emit('click', rate.value)
}

function out() {
    for (let i = 0; i < 5; i++) {
        stars.value[i].classList.remove('active', 'near');
        stars.value[i].style.fill = "";
    }
    for (let i = 0; i < rate.value; i++) {
        stars.value[i].classList.add('active');
        stars.value[i].style.fill = `hsl(${rate.value * 20}, 100%, 65%)`;

    }
}

</script>

<style scoped>

.star {
    fill: rgba(83, 91, 242, 0.2);
    margin: 5px;
    stroke: #1a1a1a;
    stroke-width: .5;
    transition: transform 100ms ease-out, fill 100ms linear;
}

.star:hover {
    fill: rgba(83, 91, 242, 1);
    transform: scale(125%) translateY(-15px);
}

.near {
    transform: scale(110%) translateY(-5px);
}

.stars {
    display: flex;
}
.stars-result {
    position: absolute;
}

.stars-result svg {
    stroke: #1a1a1a;
    stroke-width: .5;
    margin: 5px;
}

.clickAnimation {
    animation: 250ms linear 0s;
    animation-name: click;
}
.disabled {
    position: absolute;
    z-index: 2;
    width: 100%;
    height: 100%;
}

@keyframes click {
    0% {
        transform: translateY(0%);
    }
    50% {
        transform: translateY(-30px);
    }
    75% {
        transform: translateY(30px);
    }
    100% {
        transform: translateY(0%);
    }
}
</style>