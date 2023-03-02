<template>
    <div class="container pb-2">      
        <h3 class="text-dark fs-2">{{ owner }}</h3>
        <div class="health-bar">
            <div :style="{width: `${hpPoints}%`}" class="current-health"></div>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            owner: String,
            completeHpPoints: Number,
            currentHpPoints: Number,
        },
        computed: {
            hpPoints() {
                return this.currentHpPoints*100/this.completeHpPoints
            }
        }
    }
</script>

<style lang="scss" scoped>
.container{
    width: 100%;
    margin-bottom: 2em;
    box-shadow: 1px 1px 10px #3333336b;
    .health-bar{
        width: 100%;
        height: 30px;
        padding-inline: 5px;
        background-color: #eee2e2;

        .current-health{
            height: 100%;
            background-color: #c72020;
        }
    }
}
</style>