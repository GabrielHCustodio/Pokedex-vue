<template>
  <div class="container">
    <div class="row mt-2">

      <div class="col mb-2">
        <div class="card palco">
          <div class="card-header"></div>

          <div class="card-body bg-pokebola bg-normal">
            <div class="pokemon">
              <transition
                @after-enter="displayEvolutions = true"
                @before-leave="displayEvolutions = false"
              >
                <img :src="require(`@/assets/pokemons/${pokemon.image}`)" v-if="display">
              </transition>

              <div class="evolutions">
                <transition name="fade">
                  <img src="@/assets/pokemons/003.png" v-if="displayEvolutions">
                </transition>
                <transition name="fade">
                  <img src="@/assets/pokemons/002.png" v-if="displayEvolutions">
                </transition>
              </div>
            </div>
          </div>

          <div class="card-footer">
            <nav class="nav nav-pills nav-fill"></nav>
            <div class="detalhes"></div>
          </div>
        </div>
      </div>
      <!-- fim lado esquerdo -->

      <!-- início lado direito -->
      <div class="col mb-2 pokedex">        
        <div class="row">
          <div class="col">
            <h1>Pokédex</h1>
          </div>
        </div>

        <div class="row">
          <div class="col">
            <select class="form-select">
              <option>Id crescente</option>
              <option>Id decrescrente</option>
              <option>De A - Z</option>
            </select>
          </div>
        
          <div class="col">
            <input type="text" class="form-control" placeholder="Pesquisar pokémon">
          </div>
        </div>

        <div class="row">
          <div class="pokedex-catalogo">

            <div v-for="p in pokemons" :key="p.id" :class="`cartao-pokemon bg-${p.type}`" @click="analyzePokemon(p)">
              <h1>{{p.id}} {{ p.name }}</h1>
              <span>{{ p.type }}</span>
              <div class="cartao-pokemon-img">
                <img :src="require(`@/assets/pokemons/${p.image}`)">
              </div>
            </div>

          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      display: false,
      displayEvolutions: false,
      pokemon: {},
      pokemons: [
        { id: 1, name: 'Bulbasaur', type: 'grama', image: '001.png', evolutions: [2,3] },
        { id: 2, name: 'Ivysaur', type: 'grama', image: '002.png', evolutions: [3] },
        { id: 3, name: 'Venusaur', type: 'grama', image: '003.png', evolutions: [] },
        { id: 4, name: 'Charmander', type: 'fogo', image: '004.png', evolutions: [5, 6] },
        { id: 5, name: 'Charmeleon', type: 'fogo', image: '005.png', evolutions: [6] },
        { id: 6, name: 'Charizard', type: 'fogo', image: '006.png', evolutions: [] },
        { id: 7, name: 'Squirtle', type: 'agua', image: '007.png', evolutions: [8,9] },
        { id: 8, name: 'Wartortle', type: 'agua', image: '008.png', evolutions: [9] },
        { id: 9, name: 'Blastoise', type: 'agua', image: '009.png', evolutions: [] },
        { id: 10, name: 'Caterpie', type: 'inseto', image: '010.png', evolutions: [11,12] },
        { id: 11, name: 'Metapod', type: 'inseto', image: '011.png', evolutions: [12] },
        { id: 12, name: 'Butterfree', type: 'inseto', image: '012.png', evolutions: [] },
        { id: 13, name: 'Weedle', type: 'inseto', image: '013.png', evolutions: [14,15] },
        { id: 14, name: 'Kakuna', type: 'inseto', image: '014.png', evolutions: [15] },
        { id: 15, name: 'Beedrill', type: 'inseto', image: '015.png', evolutions: [] },
        { id: 16, name: 'Pidgey', type: 'normal', image: '016.png', evolutions: [17,18] },
        { id: 17, name: 'Pidgeotto', type: 'normal', image: '017.png', evolutions: [18] },
        { id: 18, name: 'Pidgeot', type: 'normal', image: '018.png', evolutions: [] }
      ]
    }
  },
  methods: {
      analyzePokemon(p) {

        if((this.pokemon.id != p.id) && this.display) {
          setTimeout( () => {
            this.analyzePokemon(p)
          }, 1000)
        }

        this.pokemon = p
        this.display = !this.display
      }
  }
}
</script>

<style>
body {
  background-color: #dee3eb;
}
</style>

<style scoped>
@import '~@/assets/css/animations.css';

.pokedex {
  padding: 20px;
  background-color: #ffffff;
  -webkit-box-shadow: 2px 2px 10px rgba(200, 200, 200, 0.77);
  -moz-box-shadow: 2px 2px 10px rgba(200, 200, 200, 0.77);
  box-shadow: 2px 2px 10px rgba(200, 200, 200, 0.77);
  border-radius: 10px;
}

.pokedex-catalogo {
  overflow: auto;
  display: flex;
  flex-wrap: wrap;
  height: 400px;
  width: 98%;
  margin-top: 10px;
}

.cartao-pokemon {
  position: relative;
  margin: 5px;
  width: 150px;
  height: 115px;
  cursor: pointer;
  border-radius: 5px;
  -webkit-box-shadow: 2px 2px 2px rgba(200, 200, 200, 0.77);
  -moz-box-shadow: 2px 2px 2px rgba(200, 200, 200, 0.77);
  box-shadow: 2px 2px 2px rgba(200, 200, 200, 0.77);
}

.cartao-pokemon h1{
  color:#fff;
  font-size: 14px;
  margin: 5px 0px 0px 5px;
  padding: 0px;
}

.cartao-pokemon span{
  color:#fff;
  position: absolute;
  background: rgba(255, 255, 255, 0.3);
  font-size: 12px;
  margin: 10px 0px 0px 5px;
  padding: 5px 10px 5px 10px;
  border-radius: 25px;
}

.cartao-pokemon img {
    max-width:60%;
    max-height:60%;
    float: right;
}

.bg-grama {
  background-color: #2d8f78;
}

.bg-fogo {
  background-color: #e47373
}

.bg-agua {
  background-color: #5a9ed2
}

.bg-inseto {
  background-color: #26d3ab
}

.bg-normal {
  background-color: #cecece
}

.bg-pokebola {
  background-image: url("~@/assets/pokebola.png");
  background-repeat: no-repeat;
  background-position: bottom right;
}

.palco {
  color: #fff;
  background-color: #333;
  -webkit-box-shadow: 2px 2px 10px rgba(230, 223, 223, 0.77);
  -moz-box-shadow: 2px 2px 10px rgba(230, 223, 223, 0.77);
  box-shadow: 2px 2px 10px rgba(230, 223, 223, 0.77);
  border-radius: 10px;
}

.pokemon {
  display: block;
  text-align: center;
  height: 215px;
}

.detalhes {
  margin: 20px 30px 20px 30px;
}

.evolutions {
  position: absolute;
  top: 0px;
  right: 0px;
  height: 70px;
}

.evolutions img {
  cursor: pointer;
  max-width: 100%;
  max-height: 100%;
  float: right;
}

</style>