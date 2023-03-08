<template>
  <div class="container">
    <h1>Bets</h1>
    <div class="row">
      <div class="col-md-12">
        <b>Current Balance</b>: {{ balance }} Eur
      </div>
    </div>
  <div class="col-md-12">
    <div class="row mt-10 p-10 table-bordered" style="padding: 10px;" v-for="event in events">
      <div class="col-md-4 text-center">
        <a @click="selectTeam(event.Id, event.Team1)" class="btn btn-info" >{{ event.Team1}}</a>
      </div>
      <div class="col-md-4 text-center">
      <a class="btn btn-default" @click="selectTeam(event.Id, 'Draw')">Draw</a>
      </div>
      <div class="col-md-4 text-center">
      <a  @click="selectTeam(event.Id, event.Team2)" class="btn btn-danger">{{ event.Team2 }}</a>
      </div>
    </div>
    <div class="mt-10" v-show="teamsSelected">
      Bet for: {{ selectedTeam }} <br/>
      <input style="margin-top: 10px; width: 100%;" class="mt-10" v-model="amount"/>
      <input @click="placeBet()" type="submit" style="margin-top: 10px; width: 100%;" class="btn btn-success mt-10" value="Place Bet"/>
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
      events: {},
      selectedEvent: 0,
      selectedTeam: "",
      teamsSelected: false,
      amount: 0
    }
  },
  mounted() {
    if (!localStorage.getItem('token')) {
      this.$router.push('/');
    }
    axios.get("https://localhost:7200/BetEvents/apigetall").then(r => {
      this.events = r.data;
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
