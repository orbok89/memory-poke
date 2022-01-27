<template>
  <div id="app" >
      <div class="container">
        <div class="card" v-for="(pokemon, index) in vettore_nuovo" :key="index">
          <div @click="reveal(index)" class="retro" v-bind:class="{ rivelato: pokemon.retro,  }">
            <img class="resize-retro" src="https://archives.bulbagarden.net/media/upload/1/17/Cardback.jpg" alt="">
          </div>
          <div class="antiretro">
            {{pokemon.name}}
            <img class="resize-antiretro"  @click="reveal()" :src="pokemon.photo" alt="">
          </div>
      </div>
    </div>
     
    <button @click="pacchetto()">gioca!</button>
    <div>vite:{{lives}}</div>
    <div>punti:{{point}}</div>
     
     
  </div>
</template>

<script>
 
import axios from 'axios';
export default {
  name: 'App',
  components: {
   
  },
  data(){
    return{
      pokedex:[],
      immagine_pokemon:[],
      vettore_pokemon:[],
      vettore_nuovo:[],
      prova1:'',
      temp:-1,
      lives:5,
      point:0
    }
  },
  mounted(){
     this.random_pokedex(),
    this.prova()
    this.prova()
  },
  
  methods:{
    pacchetto(){
    this.random_pokedex(),
    this.prova()
    this.prova()
    this.randomizzatore(this.pokedex) 
    },
    //il memory
    reveal(index){
       
       
      if(this.prova1==''){
        this.prova1=this.vettore_nuovo[index].name
        this.vettore_nuovo[index].retro=true;
       this.temp = index
         
      }
      else {
        if(this.prova1==this.vettore_nuovo[index].name){
          this.vettore_nuovo[index].retro=true;
          this.prova1=''
          this.point=this.point+1
          if(this.point==4){
            alert('bravo');
            this.pokedex=[],
            this.immagine_pokemon=[],
            this.vettore_pokemon=[],
            this.vettore_nuovo=[],
            this.prova1=''
            this.temp=-1,
            this.lives=5
            this.point=0

          }
        }
        else{
          this.vettore_nuovo[index].retro=true;
          setTimeout(()=>{this.flip_back(index,this.temp)},1000)
          this.lives=this.lives-1;

          this.prova1=''
          if(this.lives==0){
            alert('hai perso')
            this.pokedex=[],
            this.immagine_pokemon=[],
            this.vettore_pokemon=[],
            this.vettore_nuovo=[],
            this.prova1='',
            this.temp=-1,
            this.lives=5
            this.point=0
          }
        }
      }
      

    },
    // tempo di attesa per far girare le carte
    flip_back(index,temp){
       
      this.vettore_nuovo[index].retro=false;
      this.vettore_nuovo[temp].retro=false;
    },

    //chiamata axios per avere i pokomen
    prova(){
      for (var i=0; i<=4;i++){
        axios.get('https://pokeapi.co/api/v2/pokemon/'+this.vettore_pokemon[i]).then(res =>{
                console.log(res.data.sprites.front_default )
                var pokemo={
                  name:res.data.name,
                  photo:res.data.sprites.front_default,
                  retro:false
                }
                 
                this.pokedex.push(pokemo) 
            });


      }
       
       
    },
    random_pokedex(){
      for (var i= 0;i<= 3;i++){
        
        this.vettore_pokemon.push(Math.floor(Math.random() * 151))
      }
     
    },
    randomizzatore(vettore_da_randomizzare){
      this.vettore_nuovo=[]
      var i=0;
      
      var vettore_controllo=[]
      while (  i<8){
          var nuovo_index=Math.floor(Math.random()*8);
           
          
          this.is_array(vettore_controllo,nuovo_index );
          if (this.is_array(vettore_controllo,nuovo_index )==false){
            this.vettore_nuovo.push(vettore_da_randomizzare[nuovo_index]);
            vettore_controllo.push(nuovo_index);
            i++;
          }
          
      }
       
    },
    is_array(vettore, elemento){
      var flag = false;
      for (var i = 0; i < vettore.length; i++){
        if (vettore[i] == elemento){
            flag = true;
            
        }
      }
      return flag;
      }
    }
}
</script>

<style lang="scss">
  .rivelato{
    display: none;
  }
  .card{
    text-align: center;
    display: flex;
    position: relative;
    flex-direction: column;
    width: 100px;
    height: 140px;
    background-color: aqua;
    margin: 2px;
  }
  .resize-antiretro{
    width:100px;
    height: 100px;
  }
  .container{
    display: flex;
    flex-wrap: wrap;
    width: 600px;
  }
  .retro{
     
    position: absolute;
    width: 100%;
    height: 100%;
  }
  .resize-retro{
    width: 100%;
    
  }
   
</style>
