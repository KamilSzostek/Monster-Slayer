<template>
  <div :id="owner" class="container pb-2" ref="health">
    <h3 class="text-dark fs-2">{{ owner }}</h3>
    <div class="health-bar">
      <div :style="{ width: `${healthPoints}%` }" class="current-health"></div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    owner: {
      type: String,
      required: true
    },
    completeHp: Number,
    currentHp: Number
  },
  computed: {
    healthPoints() {
      return (this.currentHp * 100) / this.completeHp
    }
  }
}
</script>

<style lang="scss" scoped>
.container {
  width: 100%;
  margin-bottom: 2em;
  box-shadow: 1px 1px 10px #3333336b;
  .health-bar {
    width: 100%;
    height: 30px;
    padding-inline: 5px;
    background-color: #eee2e2;

    .current-health {
      height: 100%;
      background-color: #c72020;
    }
  }
}
</style>
