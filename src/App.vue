<script>
export default {
  data() {
    return {
      duration: 5 * 1000,
      elapsed: 0,
      stopped: false
    };
  },
  created() {
    this.stopped = false;
    let lastTime = performance.now();
    const update = () => {
      const time = performance.now();
      this.elapsed += Math.min(time - lastTime, this.duration - this.elapsed);
      lastTime = time;
      this.handle = requestAnimationFrame(update);
    };
    update();
    
  },
  unmounted() {
    cancelAnimationFrame(this.handle);
    this.stopped = true;
  },
};
</script>

<template>
  <h1 style="bottom: 50px">Elapsed Time Project</h1>
  <div :style= "elapsed / duration!=1 ? 'display: none' : ''">
    <img src="https://upload.wikimedia.org/wikipedia/commons/8/81/Stop_sign.png" width=200 style="padding-bottom: 50px">
  </div>
  <label
    >Elapsed Time:
    <progress :value="elapsed / duration"></progress>
  </label>
  <br />
  <div>{{ (elapsed / 1000).toFixed(1) }} seconds</div>
  <br />
  <div>
    Decide duration:
    <input type="range" v-model="duration" min="1" max="10000" />
    {{ (duration / 1000).toFixed(1) }} seconds
  </div>
  <br />
  <button @click="elapsed = 0">Reset elapsed time</button>
</template>
