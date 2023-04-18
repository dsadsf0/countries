<template>
  <div :class="$style.container">
    <div :class="[$style.side, $style.side_left]" @click="prev">
      Prev
    </div>
    <div :class="[$style.side, $style.side_right]" @click="next">
      Next
    </div>
    <div :class="$style.main" ref="mainNode">
      <div :class="$style.wrapper" :style="{ backgroundImage: `flags/${cArr[count].code.toLocaleLowerCase()}.svg` }">
      </div>
    </div>
    <div :class="[$style.footer, { [$style._active]: isActive }]" @click="setActive">
      <p>
        Country: <span>{{ cArr[count].name }}</span>,
        Capital: <span>{{ cArr[count].capital }}</span>
      </p>
    </div>
  </div>
</template>
<script setup lang="ts">
import countries from './countries.json'
import { ref, onMounted, onUnmounted } from 'vue';

const cArr = countries.sort(() => {
  return Math.random() - 0.5;
})

const count = ref(0);
const isActive = ref(false);
const imgNode = ref(null)
const mainNode = ref(null)
const width = ref('100%')

function checkWidth() { 
  if (imgNode && mainNode && imgNode.value && mainNode.value) {
    const main = mainNode.value as HTMLDivElement
    const img = imgNode.value as HTMLDivElement
    if (img.offsetWidth === 0 && img.offsetHeight === 0) {
      if (main.offsetWidth > main.offsetHeight - 40) {
        width.value = 'auto'
        return
      } else {
        width.value = '100%'
        return
      }
    }
    if (main.offsetWidth - 40 > img.offsetWidth && main.offsetHeight - 40 >= img.offsetHeight) {
      width.value = 'auto'
      return
    }
    if (main.offsetWidth - 40 <= img.offsetWidth && main.offsetHeight - 40 >= img.offsetHeight) {
      width.value = '100%'
      return
    }
  }
  width.value = 'auto'
}

function setActive() {
  isActive.value = !isActive.value  
}

function next() {
  if (count.value < cArr.length - 1) {
    count.value++
  } else {
    count.value = 0
  } 
  isActive.value = false
}

function prev() {
  if (count.value > 0) {
    count.value--
  } else {
    count.value = cArr.length - 1 
  }
  isActive.value = false
}

onMounted(() => {
  // checkWidth()
  // window.addEventListener('resize', checkWidth)
})

onUnmounted(() => {
  // window.removeEventListener('resize', checkWidth)
})
</script>
<style module>
.container {
  display: grid;
  grid-template-areas: "side-l main side-r" "footer footer footer";
  grid-template-columns: auto 1fr auto;
  grid-template-rows: 1fr auto;
  width: 100%;
  height: 100%;
  color: #fff;
}
.side {
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  background-color: rgba(0, 0, 0, 0.9);
  padding: 20px;
}
.side:hover {
  background-color: rgba(0, 0, 0, 0.7);
}
.side_left {
  grid-area: side-l;
}
.side_right {
  grid-area: side-r;
}

.main {
  grid-area: main;
  overflow: hidden;
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.wrapper {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
}

.image {
  display: block;
  object-position: center;
  object-fit: contain;
  height: auto;
  aspect-ratio: inherit;
}

.footer {
  cursor: pointer;
  grid-area: footer;
  text-align: center;
  padding: 20px;
  position: relative;
  background-color: #141414;
}

.footer::after {
  content: 'Click to check answer';
  position: absolute;
  background-color: #000;
  color: #fff;
  left: 0;
  top: 0;
  right: 0;
  bottom: 0;
  padding: 20px;
  pointer-events: none;
}

.footer._active::after {
  visibility: hidden;
  opacity: 0;
}
.footer:hover::after {
  background-color: #141414;
}
</style>
