<template>
  <div class="container">
  <div class="col-md-12">
    <h1>Bets</h1>
    <div class="row mt-10 p-10 table-bordered" v-for="event in events">
      <a @click="selectTeam(event.Id, event.Team1)" class="btn btn-info" >{{ event.Team1}}</a> <a class="btn btn-default" @click="selectTeam(event.Id, 'Draw')">Draw</a> <a  @click="selectTeam(event.Id, event.Team2)" class="btn btn-danger">{{ event.Team2 }}</a>
    </div>
    <div class="mt-10" v-show="teamsSelected">
      Bet for: {{ selectedTeam }} <br/>
      <input v-model="amount"/> <br/>
      <input @click="placeBet()" type="submit" class="btn btn-success" value="Place Bet"/>
    </div>
  </div>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  data(){
    return {
      events: {},
      selectedEvent: 0,
      selectedTeam: "",
      teamsSelected: false,
      amount: 0
    }
  },
  mounted() {
    axios.get("https://localhost:7200/BetEvents/apigetall").then(r => {
      this.events = r.data;
    })
  },
  methods: {
    selectTeam(eventId, TeamName){
      this.selectedTeam = TeamName;
      this.selectedEvent = eventId;
      this.teamsSelected = true;
    },

    placeBet(){
      var token = localStorage.getItem('token');
      axios.post("https://localhost:7200/BetEvents/apipostbet", {
        EventId: this.selectedEvent,
        Token: token,
        amount: this.amount,
        WinningTeam: this.selectedTeam,
      }).then(r => {
        this.$router.push('/betting-history');
      });
    }
  }
}
</script>
<style>
</style>
