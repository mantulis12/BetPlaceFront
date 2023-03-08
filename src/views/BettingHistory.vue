<template>
  <div class="container">
    <h1>Bets</h1>
    <div class="row">
      <div class="col-md-12">
        <b>Current Balance</b>: {{ balance }} Eur
      </div>
    </div>
    <div class="col-md-12">
      <div class="row table-bordered" style="padding: 10px; margin-top: 10px;" v-for="event in bets">
        Bet for: <b>{{ event.WiningTeam }}</b> in Event: <b>{{ event.BetEvent.Team1 }} - {{ event.BetEvent.Team2 }}</b> on <b>{{ event.BetEvent.EventEndDate }}</b>
        <b v-if="event.status == 1" style="margin-left: 10px;">Bet was won</b>
        <b v-if="event.status == 2" style="margin-left: 10px;">Bet was lost</b>
        <b v-if="event.status == 0" style="margin-left: 10px;">Bet is waiting for results</b>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  data(){
    return {
      balance: 0,
      bets: {},
    }
  },
  mounted() {
    if (!localStorage.getItem('token')) {
      this.$router.push('/');
    }
    axios.post("https://localhost:7200/Bets/apigetbets", {Token: localStorage.getItem('token')}).then(r => {
      this.bets = r.data;
    });
    axios.post("https://localhost:7200/api/getbalance", {Token: localStorage.getItem('token')}).then(r => {
      this.balance = r.data.balance;
    });
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
