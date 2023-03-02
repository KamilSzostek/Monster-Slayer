<template>
  <div class="row d-flex justify-content-center" v-if="enemyHealth > 0 && yourHealth > 0">
    <button class="col-md-5 btn btn-warning border border-1 border-dark" @click="$emit('updatedMonsterHealth', attack())">attack</button>
    <button class="col-md-5 btn btn-danger border border-1 border-dark" @click="specialAttack">special attack</button>
    <button class="col-md-5 btn btn-success border border-1 border-dark" @click="heal">heal</button>
    <button class="col-md-5 btn btn-secondary border border-1 border-dark" @click="surrender">surrender</button>
  </div>
  <div v-else>
    <h3>Game over!</h3>
    <p>{{ yourHealth > 0 ? 'You win!' : 'You lost!' }}</p>
    <button @click="newGame">Start new Game</button>
  </div>
</template>

<script>
export default {
  props: {
    enemyHealth: Number,
    yourHealth: Number,
    yourPower: Number
  },
  emits: ['updatedMonsterHealth'],
  methods: {
    attack() {
        return (this.enemyHealth - Math.floor(Math.random()*this.yourPower)).toString();     
    },
    specialAttack() {},
    heal() {},
    surrender() {},
    newGame() {}
  }
}
</script>

<style lang="scss" scoped>
div{
    gap: 10px;
    button{
        text-transform: capitalize;
        font-size: 2rem;
    }
}
</style>
