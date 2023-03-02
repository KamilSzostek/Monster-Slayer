<template>
  <div class="battle-log">
    <h3 class="p-5 fs-2 fw-bold">Battle Log</h3>
    <ul>
      <li v-for="log in logs" key="Math.random()">
          <span :class=" log.executor === 'Player' ? 'you' : 'monster' ">{{ `${log.executor} ` }}</span
          >{{ log.action }} and deals <span class="value">{{ log.actionValue }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    logs: []
  },
}
</script>
<style lang="scss" scoped>
.battle-log{
    min-height: 300px;
    margin-top: 5em;
    border-radius: .5em;
    text-align: center;
    box-shadow: 0 0 10px #3333335d;

    ul{
        list-style: none;
        li{
            font-size: 1.4rem;
        }
        span{
            font-weight: bold;
        }
    }
    .you {
      color: green;
    }
    .monster {
      color: royalblue;
    }
    .value{
        color: greenyellow;
    }
}
</style>
