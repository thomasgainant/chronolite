<script setup lang="ts">
import { onMounted, ref, type Ref } from "vue";
import Clock from "./components/Clock.vue";
import Footer from "./components/Footer.vue";

const rootEl = ref<HTMLElement | null>(null);
const clockRef = ref<InstanceType<typeof Clock> | null>(null);

const started = ref(false);
const tickOrTack = ref(true);

const startDateTime:Ref<Date | undefined> = ref<Date | undefined>(undefined);

function tick(){
  tickOrTack.value = !tickOrTack.value;
  console.log(tickOrTack.value);

  if(rootEl.value){
    if(started.value && tickOrTack.value === true){
      rootEl.value.className = 'tick';
    }
    else if(started.value && tickOrTack.value === false){
      rootEl.value.className = 'tack';
    }

    clockRef.value?.formatContent();
  }
}

function handleClickOnClock(): void {
  console.log("start");
  started.value = true;
  startDateTime.value = new Date();
}

onMounted(() => {
  setInterval(tick, 1000);
});
</script>

<template>
  <div id="clock-wrapper" ref="rootEl">
    <Clock ref="clockRef" :startDateTime="startDateTime" @clock-click="handleClickOnClock"></Clock>
  </div>
  <Footer></Footer>
</template>

<style scoped>
</style>
