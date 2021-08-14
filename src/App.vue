<template>
  <div id="app">
     <section class="row">
        <div class="small-6 columns">
            <h1 class="text-center">SEN</h1>
            <div class="healthbar">
                <div :style="{width : player_heal + '%'}" class="healthbar text-center" style="background-color: green; margin: 0; color: white;">
                    {{player_heal}}%
                </div>
            </div>
        </div>
        <div class="small-6 columns">
            <h1 class="text-center">CANAVAR</h1>
            <div class="healthbar">
                <div :style="{width : monster_heal + '%'}" class="healthbar text-center" style="background-color: green; margin: 0; color: white;">
                    {{monster_heal}}%
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
    <section class="row log" v-if="game_logs.length > 0">
        <div class="small-12 columns">
            <ul>
                <li 
                :class="{'player-turn' : logs.turn== 'P', 'monster-turn' : logs.turn == 'M'}"
                v-for="logs in game_logs" :key="logs">
                        {{logs.text}}
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
        game_is_on : false,
        game_logs : [],
    }
  },
  
  methods : {

      startGame : function(){
        this.game_is_on = true;
      },

      attack : function(){
        var point = Math.ceil(Math.random() * 10);
        this.monster_heal -= point;
        this.add_to_log({turn : "P", text: "Oyuncu Atagi! (" + point + ")"});
        this.monsterAttack();
      },

      specialAttack : function(){
        var point = Math.ceil(Math.random() * 25);
        this.monster_heal -= point;
        this.add_to_log({turn : "P", text: "Ozel Oyuncu Atagi! (" + point + ")"});
        this.monsterAttack();
      },

      healUp : function(){
        var point = Math.ceil(Math.random() * 15);
        this.player_heal += point;
        this.add_to_log({turn : "P", text: "Ilk Yardim (" + point + ")"});
      },

      giveUp : function(){
        this.player_heal = 0;
        this.add_to_log({turn : "P", text: "Oyuncu Pes Etti!!!"});
      },

      monsterAttack : function(){
        var point = Math.ceil(Math.random() * 15);
        this.player_heal -= point;
        this.add_to_log({turn : "M", text: "Canavar Atagi! (" + point + ")"});
      },

      add_to_log : function(logs){
          this.game_logs.push(logs);
      }

  },

  watch : {
      player_heal : function(value){
          if(value <= 0){
              this.player_heal = 0;
              if(confirm("OYUNU KAYBETTIN!")){
                  this.player_heal = 100;
                  this.monster_heal = 100;
                  this.game_logs=[];
              }
          } else if(value >= 100){
              this.player_heal = 100;
          }
      },

       monster_heal : function(value){
          if(value <= 0){
              this.monster_heal = 0;
               if(confirm("OYUNU KAZANDIN!")){
                  this.player_heal = 100;
                  this.monster_heal = 100;
                  this.game_logs=[];
              }
          }
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
