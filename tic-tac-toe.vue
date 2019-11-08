<template>
  <div class="hello">
  <div style="float: left">
      <span v-if="winner">*** {{winner}} ***</span>
      <br>
  <button v-if="stuck || winner" @click.prevent="inputs=[]">  Play Again</button>
  </div>
  <table style="clear: both">
    <caption v-if="!winner">Turn of Player {{ this.turn.player1 ? 1:2}}</caption>
    <tbody>
      <tr v-for="x in 3">
        <td v-for="y in 3" @click="dispact(x,y)">{{determine(x,y) }}</td>
      </tr>
    </tbody>
  </table>
  </div>
</template>

<script>
export default {
  name: 'tic',
  computed: {
    winner: function(){
      if(this.inputs.length >4){
        return this.turn.player1 ? this.checkForPlayer(2) : this.checkForPlayer(1)
      }
    },
    stuck: function(){
      return this.inputs.length > 8
    }
  },
  methods: {
    determine: function(x,y){
      var determ = _.find(this.inputs,function(n){
          return n.X==x && n.Y == y
      },'Player')
      if(determ){
          return determ.Player == 1 ? "O" : "X"
      }
      return ""
    },
    checkForPlayer: function(player){
        var playerInputs =  _.filter(this.inputs, function(n){
          return n.Player == player
        })
        if(this.FirstDiagnal(playerInputs) || this.SecondDiagonal(playerInputs) || this.uniwise(playerInputs)){
          return "Winner is player "+ player
        }
        return false
    },
    FirstDiagnal: function(playerInputs){
      return _.filter(playerInputs, function(n){
          return n.X==n.Y
        }).length == 3
    },
    SecondDiagonal: function(playerInputs){
      return _.filter(playerInputs, function(n){
          return (n.X==2 && n.Y==2) || (n.X==1 && n.Y==3) || (n.X==3 && n.Y==1)
      }).length == 3
    },
    uniwise: function(playerInputs){
       return _.filter([1,2,3], function(n){
        return _.filter(playerInputs, function(p){
            return p.X == n
        }).length == 3 || _.filter(playerInputs, function(p){
            return p.Y == n
        }).length == 3
       }).length > 0
    },
    dispact: function(x,y){
      if(this.doValidClick(x,y))
        this.changeTurn()
    },
    changeTurn: function(){
        this.turn.player1 = !this.turn.player1
        this.turn.player2 = !this.turn.player2
    },
    doValidClick: function(x,y,player){
      if(this.hasAlreadyBeenCLicked(x,y))
        return false;
      return this.inputs.push({X:x,Y:y,Player:this.turn.player1 ? 1:2});
    },
    hasAlreadyBeenCLicked: function(x,y){
      return _.find(this.inputs, function(n){
        return n.X==x && n.Y==y
      });
    }
  },
    data: function(){
      return {
          turn:{
            player1:true,
            player2:false
          },
          inputs:[],
          winner:false,
          }
        }
      }
</script>


<style scoped>
.hello{
  margin-top: 10%;
  margin-left: 45%;
}
.unclickable{
}
td{
  height: 30px;
  width:30px;
}
  tr>td:last-child{
border-left:2px solid black;
} tr>td:first-child{
border-right:2px solid black;
}
tbody>tr:first-child>td{
  border-bottom: 2px solid black;
}
tbody>tr:last-child>td{
  border-top: 2px solid black;
}
</style>
