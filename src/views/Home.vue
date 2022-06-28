<template>
  <div class="container">
    <div class="row mt-2">
      <div class="col mb-2">
        <div class="card palco">
          <div class="card-header"></div>

          <div class="card-body bg-pokebola bg-normal">
            <div class="pokemon">
              <transition
                name="slide"
                @after-enter="displayEvolutions = true"
                @before-leave="displayEvolutions = false"
              >
                <img
                  :src="require(`@/assets/pokemons/${pokemon.imagem}`)"
                  v-if="display"
                />
              </transition>

              <div class="evolutions">
                <transition name="fade" v-for="e in pokemon.evolucoes" :key="e">
                  <img
                    :src="
                      require(`@/assets/pokemons/${e
                        .toString()
                        .padStart(3, '0')}.png`)
                    "
                    v-if="displayEvolutions"
                  />
                </transition>
              </div>
            </div>
          </div>

          <div class="card-footer">
            <nav class="nav nav-pills nav-fill">
              <router-link
                class="nav-item nav-link text-white"
                :to="{ path: '/sobre' }"
                exact-active-class="active"
              >
                Sobre
              </router-link>
              <router-link
                class="nav-item nav-link text-white"
                :to="{ path: '/status' }"
                exact-active-class="active"
              >
                Status
              </router-link>
              <router-link
                class="nav-item nav-link text-white"
                :to="{ path: '/habilidades' }"
                exact-active-class="active"
              >
                Habilidades
              </router-link>
            </nav>

            <div class="detalhes">
              <router-view
                v-slot="{ Component }"
                :pokemon="pokemon"
                @adicionarHabilidade="adicionarHabilidade"
                @removerHabilidade="removerHabilidade"
              >
                <transition
                  enter-active-class="animate__animated animate__zoomInDown"
                >
                  <component :is="Component" />
                </transition>
              </router-view>
            </div>
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
            <select class="form-select" v-model="ordenacao">
              <option value="" disabled>Ordenar pokemons...</option>
              <option value="1">Id crescente</option>
              <option value="2">Id decrescrente</option>
              <option value="3">De A - Z</option>
              <option value="4">De Z - A</option>
            </select>
          </div>

          <div class="col">
            <input
              type="text"
              class="form-control"
              placeholder="Pesquisar pokémon"
              v-model="nomePokemon"
            />
          </div>
        </div>

        <div class="row">
          <div class="pokedex-catalogo">
            <transition-group name="ordenacao">
              <div
                v-for="p in pokemons"
                :key="p.id"
                :class="`cartao-pokemon bg-${p.tipo}`"
                @click="analyzePokemon(p)"
              >
                <h1>{{ p.id }} {{ p.nome }}</h1>
                <span>{{ p.tipo }}</span>
                <div class="cartao-pokemon-img">
                  <transition
                    appear
                    enter-active-class="animate__animated animate__fadeInDown"
                  >
                    <img :src="require(`@/assets/pokemons/${p.imagem}`)" />
                  </transition>
                </div>
              </div>
            </transition-group>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      display: false,
      displayEvolutions: false,
      pokemon: {},
      pokemons: [],
      ordenacao: '',
      nomePokemon: ''
    };
  },
  created() {
    fetch("http://localhost:3000/pokemons")
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        this.pokemons = data;
      });
  },
  methods: {
    analyzePokemon(p) {
      let mudarPokemon = false;

      if (this.pokemon.id != p.id && this.display) {
        setTimeout(() => {
          this.analyzePokemon(p);
        }, 1000);
        mudarPokemon = true;
      }

      this.pokemon = p;
      this.display = !this.display;
      this.displayEvolutions = !this.displayEvolutions;

      if (!this.display && !mudarPokemon) {
        this.pokemon = {};
      }
    },
    adicionarHabilidade(habilidade) {
      if (this.pokemon.habilidades) {
        this.pokemon.habilidades.push(habilidade);
      }
    },
    removerHabilidade(indice) {
      if (this.pokemon.habilidades) {
        this.pokemon.habilidades.splice(indice, 1);
      }
    },
  },
  watch: {
    ordenacao(value) {
      if(value == 1) {
        this.pokemons.sort((a,b) => {
          if(b.id < a.id) {
            return 1
          }else if(b.id > a.id) {
            return -1
          }
          return 0
        })
      }
      if(value == 2) {
        this.pokemons.sort((a,b) => {
          if(b.id < a.id) {
            return -1
          }else if(b.id > a.id) {
            return 1
          }
          return 0
        })
      }
      if(value == 3) {
        this.pokemons.sort((a,b) => {
          if(b.nome < a.nome) {
            return 1
          }else if(b.nome > a.nome) {
            return -1
          }
          return 0
        })
      }
      if(value == 4) {
        this.pokemons.sort((a,b) => {
          return b.nome.localeCompare(a.nome) 
        })
      }
    },
    nomePokemon(value) {
      fetch(`http://localhost:3000/pokemons?nome_like=${value}`)
      .then((response) => {
        return response.json();
      })
      .then((data) => {
        this.pokemons = data;
      });
    }
  },
};
</script>

<style>
body {
  background-color: #dee3eb;
}
</style>

<style scoped>
@import "~@/assets/css/animations.css";

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

.cartao-pokemon h1 {
  color: #fff;
  font-size: 14px;
  margin: 5px 0px 0px 5px;
  padding: 0px;
}

.cartao-pokemon span {
  color: #fff;
  position: absolute;
  background: rgba(255, 255, 255, 0.3);
  font-size: 12px;
  margin: 10px 0px 0px 5px;
  padding: 5px 10px 5px 10px;
  border-radius: 25px;
}

.cartao-pokemon img {
  max-width: 60%;
  max-height: 60%;
  float: right;
}

.bg-grama {
  background-color: #2d8f78;
}

.bg-fogo {
  background-color: #e47373;
}

.bg-agua {
  background-color: #5a9ed2;
}

.bg-inseto {
  background-color: #26d3ab;
}

.bg-eletrico {
  background-color: #9b9b00;
}

.bg-normal {
  background-color: #cecece;
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
}
</style>
