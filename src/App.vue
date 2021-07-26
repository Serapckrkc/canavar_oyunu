<template>
  <div id="app">
     <section class="row">
        <div class="small-6 columns">
            <h1 class="text-center">SEN</h1>
            <div class="healthbar">
                <div class="healthbar text-center" style="background-color: green; margin: 0; color: white;">

                </div>
            </div>
        </div>
        <div class="small-6 columns">
            <h1 class="text-center">CANAVAR</h1>
            <div class="healthbar">
                <div class="healthbar text-center" style="background-color: green; margin: 0; color: white;">

                </div>
            </div>
        </div>
    </section>
    <section class="row controls" v-if="!game_is_on">
        <div class="small-12 columns">
            <button id="start-game" @click="startGame">YENİ OYUN</button>
        </div>
    </section>
    <section class="row controls" v-if="game_is_on">
        <div class="small-12 columns">
            <button id="attack" @click="attack">SALDIRI</button>
            <button id="special-attack" @click="specialAttack">ÖZEL SALDIRI</button>
            <button id="heal" @click="healUp">İLK YARDIM</button>
            <button id="give-up" @click="giveUp">PES ET!</button>
        </div>
    </section>
    <section class="row log" v-if="game_is_on">
        <div class="small-12 columns">
            <ul>
                <li>

                </li>
            </ul>
        </div>
    </section>
  </div>
</template>

<script>

export default {
  data (){
    return{
        player_heal : 100,
        monster_heal : 100,
        game_is_on : false
    }
  },
  
  methods : {

      startGame : function(){
        this.game_is_on = true;
      },

      attack : function(){
        var point = Math.ceil(Math.random() * 10);
        this.monster_heal -= point;
        this.monsterAttack();
        console.log("M : " + this.monster_heal);
        console.log("P : " + this.monster_heal);

      },

      specialAttack : function(){
        var point = Math.ceil(Math.random() * 25);
        this.player_heal += point;
        this.monsterAttack();
        console.log("M : " + this.monster_heal);
        console.log("P : " + this.player_heal);
      },

      healUp : function(){
        var point = Math.ceil(Math.random() * 15);
        this.player_heal += point;
        console.log("M : " + this.monster_heal);
        console.log("P : " + this.player_heal);
      },

      giveUp : function(){
        this.player_heal = 0;
        console.log("M : " + this.monster_heal);
        console.log("P : " + this.player_heal);
      },

      monsterAttack : function(){
        var point = Math.ceil(Math.random() * 15);
        this.player_heal -= point;
        console.log("M : " + this.monster_heal);
        console.log("P : " + this.player_heal);
      }

  }
 
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
