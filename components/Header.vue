<template>
  <div class="container mx-auto py-8">
    <FifaLogo class="mx-auto my-5" />
   
  </div>
</template>

<script>
import { jsonWorldCup } from '../utils/index'

export default {
    name: 'header',
    data() {
        return {
            tournaments: []
        }
    },
    async mounted(){
        const rawdata = await this.$axios.$get(jsonWorldCup)
        this.tournaments = JSON.parse(rawdata).tournaments
    }
}
</script>

<style>

</style>