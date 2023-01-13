<template>
  <div class="container mx-auto grid gap-y-4 py-10">
      <div>
        #Match
      </div>
      <div class="grid grid-cols-1 gap-y-5 lg:grid lg:grid-cols-2 lg:gap-x-10 ">
        <div class="bg-slate-800 pt-8 shadow-lg rounded-lg px-4">
          <template v-if="!loading.winner">
            <CreateChart :dataLabels="true" :title="'Winner of The World Cup'" :typeChart="'bar'" :dataSeries="winnerWorldCup.series" :typeXaxis="'category'" :categories="winnerWorldCup.categories" />
          </template>
          <template v-else>
            <div class="pb-8">loading...</div>
          </template>
        </div>
        <div class="bg-slate-800 pt-8 shadow-lg rounded-lg px-4">
          <template v-if="!loading.host">
          <CreateChart :dataLabels="true" :title="'World Cup Country Host'" :typeChart="'bar'" :dataSeries="hostWorldCup.series" :typeXaxis="'category'" :categories="hostWorldCup.categories" />
          </template>
          <template v-else>
            <div class="pb-8">loading...</div>
          </template>
        </div>
      </div>
      <div class="grid grid-cols-1 gap-y-5 lg:grid lg:grid-cols-2 lg:gap-x-10 ">
        <div class="bg-slate-800 pt-8 shadow-lg rounded-lg px-4">
          <template v-if="!loading.teamWinLose">
            <CreateChart :dataLabels="true" :title="'The Country With The highest Winrate (%) (top 20)'" :typeChart="'bar'" :dataSeries="teamWinLose.series" :typeXaxis="'category'" :categories="teamWinLose.categories" />
          </template>
          <template v-else>
            <div class="pb-8">loading...</div>
          </template>
        </div>
        <div class="bg-slate-800 pt-8 shadow-lg rounded-lg px-4">
          <template v-if="!loading.host">
            <div class="flex flex-col items-center justify-center h-full">
              <div class="text-xl">FIFA World Cup Host Winrate To Win The World Cup</div>
              <div class="text-4xl font-bold">{{ parseFloat(rateHost).toFixed(2) }} %</div>
            </div>
          </template>
          <template v-else>
            <div class="pb-8">loading...</div>
          </template>
        </div>
      </div>

      <!-- goals -->
      <div>
        #Goals
      </div>
      <div class="grid grid-cols-1 gap-y-5 lg:grid lg:grid-cols-2 lg:gap-x-10 ">
        <div class="bg-slate-800 pt-8 shadow-lg rounded-lg px-4">
          <template v-if="!loading.goal">
            <CreateChart :dataLabels="true" :title="'Total Goal Per World Cup'" :typeChart="'area'" :dataSeries="goalWorldCup.series" :typeXaxis="'category'" :categories="goalWorldCup.categories" />
          </template>
          <template v-else>
            <div class="pb-8">loading...</div>
          </template>
        </div>
        <div class="bg-slate-800 pt-8 shadow-lg rounded-lg px-4">
          <template v-if="!loading.host">
          <CreateChart :dataLabels="true" :title="'Goal Distributed'" :typeChart="'bar'" :dataSeries="distributedGoal.series" :typeXaxis="'category'" :categories="distributedGoal.categories" />
          </template>
          <template v-else>
            <div class="pb-8">loading...</div>
          </template>
        </div>
      </div>
      <div class="grid grid-cols-1 gap-y-5 lg:grid lg:grid-cols-2 lg:gap-x-10 ">
        <div class="bg-slate-800 pt-8 shadow-lg rounded-lg px-4">
          <template v-if="!loading.countryGoal">
            <CreateChart :dataLabels="true" :title="'Total Goal Per Country (Top 20)'" :typeChart="'bar'" :dataSeries="countryGoal.series" :typeXaxis="'category'" :categories="countryGoal.categories" />
          </template>
          <template v-else>
            <div class="pb-8">loading...</div>
          </template>
        </div>
        <div class="bg-slate-800 pt-8 shadow-lg rounded-lg px-4">
          <template v-if="!loading.playerGoal">
            <CreateChart :dataLabels="true" :title="'Total Goal Per Player (Top 20)'" :typeChart="'bar'" :dataSeries="playerGoal.series" :typeXaxis="'category'" :categories="playerGoal.categories" />
          </template>
          <template v-else>
            <div class="pb-8">loading...</div>
          </template>
        </div>
      </div>
      <div class="grid grid-cols-1 gap-y-5 lg:grid lg:grid-cols-1 lg:gap-x-10 ">
        <div class="bg-slate-800 pt-8 shadow-lg rounded-lg px-4">
          <template v-if="!loading.penaltyGoal">
            <CreateChart :dataLabels="true" :title="'Total Penalty Goal Per World Cup'" :typeChart="'line'" :dataSeries="penaltyGoal.series" :typeXaxis="'category'" :categories="penaltyGoal.categories" />
          </template>
          <template v-else>
            <div class="pb-8">loading...</div>
          </template>
        </div>
      </div>
      <div>#Awards</div>
      <div class="flex gap-x-4 py-4 px-1 overflow-x-auto">
        <div @click="selectedTournament = item.tournament_id" :class="{'bg-blue-800 ring-2 ring-inset': selectedTournament === item.tournament_id}" class="cursor-pointer px-6 py-2 flex bg-slate-800 rounded-xl whitespace-nowrap hover:bg-blue-800 hover:ring-2 hover:ring-inset" v-for="(item, index) in tournaments" :key="index">{{ item.tournament_id }}</div>
      </div>
      <div class="grid grid-cols-1 mt-10 gap-y-10 lg:grid lg:grid-cols-2 lg:gap-x-10 lg:gap-y-10 lg:mt-10">
        <template v-if="!loading.award">
        <div class="flex flex-col items-center justify-center bg-slate-800 rounded-lg shadow-lg h-full p-5 hover:ring-2 hover:bg-blue-800 hover:rounded-lg">
          <div class="text-xl text-white/50">Golden Ball (Best Player)</div>
          <div class="font-bold text-4xl">{{ goldenBall }}</div>
          <div>{{ goldenBall ? null : `No Award For this category in ${selectedTournament}` }}</div>
        </div>
        <div class="flex flex-col items-center justify-center bg-slate-800 rounded-lg shadow-lg h-full p-5 hover:ring-2 hover:bg-blue-800 hover:rounded-lg">
          <div class="text-xl text-white/50">Golden Boot (Top Scorer)</div>
          <div class="font-bold text-4xl">{{ goldenBoot }}</div>
          <div>{{ goldenBoot ? null : `No Award For this category in ${selectedTournament}` }}</div>
        </div>
        <div class="flex flex-col items-center justify-center bg-slate-800 rounded-lg shadow-lg h-full p-5 hover:ring-2 hover:bg-blue-800 hover:rounded-lg">
          <div class="text-xl text-white/50">Golden Glove (Best GoalKeeper)</div>
          <div class="font-bold text-4xl">{{ goldenGlove }}</div>
          <div>{{ goldenGlove ? null : `No Award For this category in ${selectedTournament}` }}</div>
        </div>
        <div class="flex flex-col items-center justify-center bg-slate-800 rounded-lg shadow-lg h-full p-5 hover:ring-2 hover:bg-blue-800 hover:rounded-lg">
          <div class="text-xl text-white/50">Best Young Player</div>
          <div class="font-bold text-4xl">{{ bestYoung }}</div>
          <div>{{ bestYoung ? null : `No Award For this category in ${selectedTournament}` }}</div>
        </div>
        </template>
        <template v-else>
          <div>loading...</div>
        </template>
      </div>
  </div>
</template>

<script>
import { jsonWorldCup } from '../utils/index'
export default {
  name: 'IndexPage',
  data(){
    return {
      dataWorldCup: {},
      winnerWorldCup: {},
      hostWorldCup: {},
      goalWorldCup: {},
      distributedGoal: {},
      countryGoal: {},
      playerGoal: {},
      penaltyGoal: {},
      teamWinLose: {},
      rateHost: 0,
      tournaments: [],
      selectedTournament : '',
      goldenBall: null,
      goldenBoot: null,
      goldenGlove: null,
      bestYoung: null,
      loading: {
        winner: true,
        host: true,
        goal: true,
        distributedGoal: true,
        countryGoal: true,
        playerGoal: true,
        penaltyGoal: true,
        teamWinLose: true,
        rateHost: true,
        award: true
      }
    }
  },
  watch: {
    selectedTournament(){
      this.getAward();
    }
  },
  async mounted(){
    const rawdata = await this.$axios.$get(jsonWorldCup)
    this.dataWorldCup = JSON.parse(rawdata)
    this.tournaments = this.dataWorldCup.tournaments
    this.selectedTournament = this.tournaments[0].tournament_id
    this.runAll()
  },
  methods: {
    runAll(){
      this.getWinnerWorldCup();
      this.getHostWorldCup();
      this.getGoalsPerWC();
      this.getDistributedGoal();
      this.getCountryGoal();
      this.getPlayerGaol();
      this.getPenaltyGoal();
      this.getTeamWinLose();
      this.getRateHostWin();
      this.getHighestTotalGoal();
      this.getAward();
    },
    async getAward(){
      this.loading.award = true
      this.goldenBall = null
      this.goldenBoot = null
      this.goldenGlove = null
      this.bestYoung = null

      await this.dataWorldCup.award_winners.forEach(element => {
        if(element.tournament_id === this.selectedTournament) {
          if(element.award_id === 'A-1') {
            this.goldenBall = `${element.given_name === 'not applicable' ? '' : element.given_name} ${element.family_name}`
          }

          if(element.award_id === 'A-4') {
            this.goldenBoot = `${element.given_name === 'not applicable' ? '' : element.given_name} ${element.family_name}`
          }

          if(element.award_id === 'A-7') {
            this.goldenGlove = `${element.given_name === 'not applicable' ? '' : element.given_name} ${element.family_name}`
          }

          if(element.award_id === 'A-8') {
            this.bestYoung = `${element.given_name === 'not applicable' ? '' : element.given_name} ${element.family_name}`
          }
        }
      })
      this.loading.award = false
    },
    async getHighestTotalGoal(){
      // let highest = -1
      // await this.dataWorldCup.team_appearances.forEach(element => {
      //   if(element.goal_differential > highest) {
      //     highest = element.goal_differential
      //   }
      // })
      //  await this.dataWorldCup.team_appearances.forEach(element => {
      //   if(element.goal_differential === highest) {
      //     console.log(element)
      //   }
      // })

    },
    async getRateHostWin() {
      this.loading.penaltyGoal = true
      await this.dataWorldCup.tournaments.forEach((element) => {
        this.rateHost += element.host_won
          
      });

      this.rateHost = (this.rateHost / this.dataWorldCup.tournaments.length) * 100
      this.loading.penaltyGoal = false
    },
    async getTeamWinLose(){
      this.loading.teamWinLose = true
      let series = []
      let categories = []
      let temp = {}
      let totalMatch = {}
      await this.dataWorldCup.team_appearances.forEach((element) => {
        if(element.team_name in totalMatch) {
          totalMatch[element.team_name] += 1
        } else {
          totalMatch[element.team_name] = 1
        }
      });

      await this.dataWorldCup.team_appearances.forEach((element) => {
        if(element.team_name in temp) {
          temp[element.team_name] += element.win
        } else {
          temp[element.team_name] = element.win
        }
      });
      for(let i in totalMatch) {
        temp[i] = parseFloat(( temp[i]/totalMatch[i] ) * 100).toFixed(2)
      }
      

      const result = Object
      .entries(temp)
      .sort((a, b) => b[1] - a[1])
      .reduce((_sortedObj, [k,v]) => ({
        ..._sortedObj, 
        [k]: v
      }), {})

      let indexGoal = 0
      for(let i in result) {
        if(indexGoal <= 20) {
          categories.push(i)
          series.push(result[i])
        }
        
        indexGoal += 1
      }
      
      this.teamWinLose = {
        series: series,
        categories: categories
      }
      this.loading.teamWinLose = false
    },
    async getPenaltyGoal(){
      this.loading.penaltyGoal = true
      let series = []
      let categories = []
      let temp = {}
      await this.dataWorldCup.goals.forEach((element) => {
        if(element.penalty === 1) {
          if(element.tournament_id in temp) {
            temp[element.tournament_id] += 1
          } else {
            temp[element.tournament_id] = 1
          }
        }
      });

      for(let i in temp) {
          categories.push(i)
          series.push(temp[i])
      }
      
      this.penaltyGoal = {
        series: series,
        categories: categories
      }
      this.loading.penaltyGoal = false
    },
    async getPlayerGaol(){
      this.loading.playerGoal = true
      let series = []
      let categories = []
      let temp = {}
      await this.dataWorldCup.goals.forEach((element) => {
        if(`${element.given_name === 'not applicable' ? '' : element.given_name} ${element.family_name}` in temp) {
          temp[`${element.given_name === 'not applicable' ? '' : element.given_name} ${element.family_name}`] += 1
        } else {
          temp[`${element.given_name === 'not applicable' ? '' : element.given_name} ${element.family_name}`] = 1
        }
      });

      const result = Object
      .entries(temp)
      .sort((a, b) => b[1] - a[1])
      .reduce((_sortedObj, [k,v]) => ({
        ..._sortedObj, 
        [k]: v
      }), {})
      let indexGoal = 0
      for(let i in result) {
        if(indexGoal <= 20) {
          categories.push(i)
          series.push(result[i])
        }
        
        indexGoal += 1
      }
      
      this.playerGoal = {
        series: series,
        categories: categories
      }
      this.loading.playerGoal = false
    },
    async getCountryGoal(){
      this.loading.countryGoal = true
      let series = []
      let categories = []
      let temp = {}
      await this.dataWorldCup.goals.forEach((element) => {
        if(element.team_name in temp) {
          temp[element.team_name] += 1
        } else {
          temp[element.team_name] = 1
        }
      });

      const result = Object
      .entries(temp)
      .sort((a, b) => b[1] - a[1])
      .reduce((_sortedObj, [k,v]) => ({
        ..._sortedObj, 
        [k]: v
      }), {})

      let indexGoal = 0
      for(let i in result) {
        if(indexGoal <= 20) {
          categories.push(i)
          series.push(result[i])
        }
        
        indexGoal += 1
      }
      
      this.countryGoal = {
        series: series,
        categories: categories
      }
      this.loading.countryGoal = false
    },
    async getDistributedGoal(){
      this.loading.distributedGoal = true
      let categories = []
      let series = []
      let temp = {
        'first-half': 0,
        'second-half': 0
      }
      await this.dataWorldCup.goals.forEach(element => {
        if(element.minute_regulation <= 45) {
          temp['first-half'] += 1
        } else {
          temp['second-half'] += 1
        }
      })

       for(let i in temp) {
        categories.push(i)
        series.push(temp[i])
      }
      
      this.distributedGoal = {
        series: series,
        categories: categories
      }

      this.loading.distributedGoal = false

    },
    async getGoalsPerWC(){
      this.loading.goal = true
      let series = []
      let categories = []
      let temp = {}

      await this.dataWorldCup.goals.forEach(element => {
        if(element.tournament_id in temp) {
          temp[element.tournament_id] += 1
        } else {
          temp[element.tournament_id] = 1
        }
      });

      for(let i in temp) {
        categories.push(i)
        series.push(temp[i])
      }
      
      this.goalWorldCup = {
        series: series,
        categories: categories
      }

      this.loading.goal = false
    },
    async getHostWorldCup(){
      this.loading.host = true
      let series = []
      let categories = []
      let temp = {}
      await this.dataWorldCup.tournaments.forEach((element) => {
        if(element.host_country in temp) {
          temp[element.host_country] += 1
        } else {
          temp[element.host_country] = 1
        }
      });

      const result = Object
      .entries(temp)
      .sort((a, b) => b[1] - a[1])
      .reduce((_sortedObj, [k,v]) => ({
        ..._sortedObj, 
        [k]: v
      }), {})

      for(let i in result) {
        categories.push(i)
        series.push(result[i])
      }
      
      this.hostWorldCup = {
        series: series,
        categories: categories
      }
      this.loading.host = false
    },
    async getWinnerWorldCup(){
      this.loading.winner = true
      let series = []
      let categories = []
      let temp = {}
      await this.dataWorldCup.tournaments.forEach((element) => {
        if(element.winner in temp) {
          temp[element.winner] += 1
        } else {
          temp[element.winner] = 1
        }
      });

      const result = Object
      .entries(temp)
      .sort((a, b) => b[1] - a[1])
      .reduce((_sortedObj, [k,v]) => ({
        ..._sortedObj, 
        [k]: v
      }), {})

      for(let i in result) {
        categories.push(i)
        series.push(result[i])
      }
      
      this.winnerWorldCup = {
        series: series,
        categories: categories
      }
      this.loading.winner = false
    }
  }
}
</script>
