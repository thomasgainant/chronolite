<template>
    <div id="clock" @click="handleClick"><span v-if="hours != '' && hours != '00'">{{ hours }} :</span><span>{{ minutes }}</span>:<span>{{ seconds }}</span></div>
</template>

<script setup lang="ts">
import { onMounted, ref, watch, watchEffect, type Ref } from 'vue';

const hours = ref("00");
const minutes = ref("00");
const seconds = ref("00");

const props = defineProps<{
  startDateTime: Date | undefined;
}>();


defineExpose({
    formatContent
})

watch(() => props.startDateTime!, (newVal) => {
  console.log('Updated value:', newVal);
});

const emit = defineEmits<{
    (e: 'clockClick'): void;
}>();

function formatContent(){
    if(!props.startDateTime)
        return;

    let now = new Date();
    let diff = now.getTime() - props.startDateTime.getTime();

    let hoursAmount = (60*60*1000);
    let hoursNumber = Math.floor(diff/hoursAmount);
    diff = diff - (hoursAmount * hoursNumber);
    
    let minutesAmount = (60*1000);
    let minutesNumber = Math.floor(diff/minutesAmount);
    diff = diff - (minutesAmount * minutesNumber);

    let secondsAmount = 1000;
    let secondsNumber = Math.floor(diff/secondsAmount);
    diff = diff - (secondsAmount * secondsNumber);

    console.log(hoursNumber+":"+minutesNumber+":"+secondsNumber);

    if(hoursNumber < 10)
        hours.value = "0"+hoursNumber;
    else
        hours.value = ""+hoursNumber;

    if(minutesNumber < 10)
        minutes.value = "0"+minutesNumber;
    else
        minutes.value = ""+minutesNumber;

    if(secondsNumber < 10)
        seconds.value = "0"+secondsNumber;
    else
        seconds.value = ""+secondsNumber;
}

function handleClick(){
    emit('clockClick');
}

onMounted(() => {
    formatContent();
});
</script>

<style>
</style>