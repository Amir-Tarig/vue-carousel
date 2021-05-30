<template>
   <div class="app">
      <div class="hello" v-for="(color, index) in colors" :key="color"> 
        <transition name="fade">
          <div :class="color" v-if="currentSlide === index"></div>
        </transition>
      </div>
      <div class="dots" >
        <div @click="makeActive(index)" class="dot" v-for="(length, index) in colors" :style="currentSlide == index ? `background: #fff` : 'background: gray' " :key="length"></div>
      </div>
   </div>

    <div class="info">
         <h1 v-if="isTitileShowing">Slider Carousel</h1>
          <button @click="isTitileShowing = !isTitileShowing">Toggle Text</button>
    </div>
</template>

<script>
import { onMounted, onUnmounted, reactive, ref } from 'vue'

export default {
  name: 'Carousel',
  setup () {
    const currentSlide = ref(0)
    const interval = ref('')
    const colors = reactive(['blue', 'yellow', 'teal'])
    const isTitileShowing = ref(true)

    const makeActive = (index) => {
      currentSlide.value = index
    }

    onMounted(() => {
      interval.value = setInterval(() => {
        currentSlide.value === 2 ? currentSlide.value = 0 : currentSlide.value++
      },5000)
    })

    onUnmounted(() => clearInterval(interval.value))

    return { currentSlide, colors , isTitileShowing, makeActive}
  }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.app {
  overflow: hidden;
  height: 350px;
  position: relative;

}
.hello {
  display: flex;
  flex-wrap: wrap;
  width: 100%;
  position: relative;
}

.hello > * {
  height: 350px;
  width: 100%;
  position: absolute;
}

.blue {
  background: blue;
}
.yellow {
  background: yellow;
}
.teal {
  background: teal;
}

.info {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.fade-enter-active,
.fade-leave-active {
  transition: all 1s ease;
}

.fade-enter-from{
  opacity: 0;
  transform: translateX(100%);
}

.fade-leave-to {
  opacity: 0;
  transform: translate(-100%);
}

.dots{
  position: absolute;
  bottom: 0;
  left: 50%;
  width: 50px;
  transform: translate(0, -50%);
  display: flex;
  justify-content: space-between;
}

.dot {
  /* background: #333; */
  width: 10px;
  height: 10px;
  border-radius: 50%;
  cursor: pointer;
}
</style>
