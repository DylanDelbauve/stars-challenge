<template>
    <svg @mouseenter="(e) => checkMousePosition(e, 'hover-in')" @mouseleave="$emit('hover-out')" @mousemove="(e) => checkMousePosition(e, 'hover-move')" @click="(e) => checkMousePosition(e, 'click')" id="star" :width="size" :height="size" :viewBox="`0 0 ${size+4} ${size+4}`">
        <mask id="starmask">
            <svg :width="size" :height="size" fill="white" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 22 20">
                <path d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z"/>
            </svg>
        </mask>
        <rect :width="`${rate*25}%`" height="100%" id="fill"></rect>
        <svg stroke-width="1" stroke="black" stroke-opacity="100%" fill="none" :width="size" :height="size" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 22 20">
            <path d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z"/>
        </svg>
    </svg>
</template>

<script setup lang="ts">

const emit = defineEmits(['hover-in', 'hover-out', 'hover-move', 'click'])
const props = defineProps({
    size: Number,
    rate: Number
});
function checkMousePosition(event, eventName: string) {
    let rect = event.target.getBoundingClientRect()
    let x = event.clientX - rect.left;
    let result = Math.round(x * 100 / props.size);
    let value = 1;
    if(result >= 75)
        value = 4;

    if(result >= 50 && result < 75)
        value = 3;

    if(result >= 25 && result < 50)
        value = 2;

    emit(eventName, value)
}
</script>

<style scoped>
  #fill {
      fill: #646cff;
      mask: url("#starmask");
  }

  #star {
      transform: translateY(0%);
      transition: transform 100ms linear;
  }

  #star:hover {
      transform: translateY(-25%);
      transition: transform 100ms linear;
  }

  #star rect {
      transition: width 200ms ease-out 200ms;
  }
</style>