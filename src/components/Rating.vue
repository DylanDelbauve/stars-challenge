<template>
    <div>
        <svg @mousemove="hover(index)" @mouseout="out" @click.prevent="click(index)" class="star" :class="{clickAnimation: onClickAnimation}" ref="stars" v-for="index in 5"
             width="64" height="64" aria-hidden="true" xmlns="http://www.w3.org/2000/svg"
             viewBox="0 0 22 20">
            <path d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z"/>
        </svg>
    </div>
</template>

<script setup lang="ts">

import {Ref, ref} from "vue";

const stars = ref([]);

const rate: Ref<Number> = ref<Number>(0);

const onClickAnimation = ref(false);

function hover(index: Number) {
    for (let i = 0; i < 5; i++) {
        stars.value[i].classList.remove('active', 'near');
        stars.value[i].style.fill = "";
    }
    for (let i = 0; i < index; i++) {
        let star = stars.value[i];
        star.classList.add('active');
        star.style.fill = getColor(index);
    }
    if (index != 1)
        stars.value[index - 2].classList.add('near');
}

function click(index: number) {
    rate.value = index;
    let delay = 300;
    stars.value.forEach((star, i) => {
        star.style.animationDelay = `${delay * i}ms`;
    });
    onClickAnimation.value = true
    setTimeout(() => {
        onClickAnimation.value = false
    }, 2000)
}

function out() {
    for (let i = 0; i < 5; i++) {
        stars.value[i].classList.remove('active', 'near');
        stars.value[i].style.fill = "";
    }
    for (let i = 0; i < rate.value; i++) {
        stars.value[i].classList.add('active');
        stars.value[i].style.fill = getColor(rate.value);

    }
}

function getColor(index: number): string {
    let color: string = "";
    switch (index) {
        case 1:
            color = "rgb(242, 120, 83)";
            break;
        case 2:
            color = "rgb(242,149,83)";
            break;
        case 3:
            color = "rgb(242,205,83)";
            break;
        case 4:
            color = "rgb(197,242,83)";
            break;
        case 5:
            color = "rgb(147,242,83)";
            break;
    }
    return color;
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

div {
    display: flex;
}

.clickAnimation {
    animation: 500ms linear 0s;
    animation-name: click;
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