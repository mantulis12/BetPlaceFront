<template>
  <div class="container">
    <div class="col-md-12">
      <h1>Bets</h1>
      <div class="row" v-for="event in bets">
        Bet for: <b>{{ event.WiningTeam }}</b> in Event: <b>{{ event.BetEvent.Team1 }} - {{ event.BetEvent.Team2 }}</b> on <b>{{ event.BetEvent.EventEndDate }}</b>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  data(){
    return {
      bets: {},
    }
  },
  mounted() {
    axios.post("https://localhost:7200/Bets/apigetbets", {Token: localStorage.getItem('token')}).then(r => {
      this.bets = r.data;
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
