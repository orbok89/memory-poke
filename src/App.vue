<template>
  <div id="app" class="container">
      <div class="container">
        <div class="card" v-for="(pokemon, index) in vettore_nuovo" :key="index">
        {{pokemon[0]}}
        <div>
          {{pokemon[0]}}
        </div>
        <div>
          
        </div>
        <img class="resize"  :src="pokemon[1]" alt="">
      </div>
    </div>
     
    <button @click="randomizzatore(pokedex)">cccc</button>
     
     
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
      vettore_nuovo:[]
    }
  },
  mounted(){
    this.random_pokedex(),
    this.prova()
    this.prova()
     
  },
  
  methods:{
    prova(){
      for (var i=0; i<=4;i++){
        axios.get('https://pokeapi.co/api/v2/pokemon/'+this.vettore_pokemon[i]).then(res =>{
                console.log(res.data.sprites.front_default )
                var pokemo=[]
                pokemo.push(res.data.name);
                pokemo.push(res.data.sprites.front_default)
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
            console.log(nuovo_index)
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
  .card{
    text-align: center;
    display: flex;
    flex-direction: column;
    width: 100px;
    height: 150px;
    background-color: aqua;
    margin: 2px;
  }
  .resize{
    width:100px;
    height: 100px;
  }
  .container{
    display: flex;
    flex-wrap: wrap;
    width: 600px;
  }
</style>
