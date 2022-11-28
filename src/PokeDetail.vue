<template>
  <div class="detail">
    <div class="detail-view card" v-if="show">
      <div v-if="pokemon" class="image">
        <img :src="imageUrl + pokemon.id + '.png'" alt="">
      </div>
      <div v-if="pokemon" class="data card-body">
        <h2 class="card-title">{{ pokemon.name }}</h2>
        <div class="property">
          <div class="left destaq">Experiencia</div>
          <div class="right">{{ pokemon.base_experience }} XP</div>
        </div>
        <div class="property">
          <div class="left destaq">Altura</div>
          <div class="right">{{ pokemon.height / 10 }} m</div>
        </div>
        <div class="property">
          <div class="left  destaq">Peso</div>
          <div class="right">{{ pokemon.weight / 10 }} kg</div>
        </div>
        <h3>Tipo</h3>
        <div class="types">
          <div class="type" v-for="(value, index) in pokemon.types" :key="'value' + index"><span :class="value.type.name">
              {{ value.type.name }}
            </span>
          </div>
        </div>
        <h3>Habilidades</h3>
        <div class="abilities">
          <div class="ability" v-for="(value, index) in pokemon.abilities" :key="'value' + index">
            {{ value.ability.name }}
          </div>
          <div class="right inactive">{{ pokemon.moves.map(item => ' ' + item.move.name).toString() }} .</div>
        </div>
      </div>
      <h2 v-else>No se encontro el pokemon indicado</h2>
      <button class="close" @click="closeDetail">Cerrar</button>
    </div>
    <i v-else class="fas fa-spinner fa-spin"></i>
  </div>
</template>
  
<script>
export default {
  props: [
    'pokemonUrl',
    'imageUrl'
  ],
  data: () => {
    return {
      show: false,
      pokemon: {}
    }
  },
  methods: {
    fetchData() {
      //Consumir la api para obtener la información de dicho pokemon
      let req = new Request(this.pokemonUrl);
      fetch(req)
        .then((resp) => {
          if (resp.status === 200)
            return resp.json(); //Almacena el resultado en un json
        })
        .then((data) => {
          this.pokemon = data; //Guardar las información del pokemon en la variable data
          this.show = true; //Mostrar la ventana emergente
        })
        .catch((error) => {
          console.log(error);
        })
    },
    closeDetail() {
      this.$emit('closeDetail');
    }
  },
  created() {
    this.fetchData();
  }
}
</script>
  
<style lang="scss" scoped>
.type {
  .fire {
    background: #FDDFDF !important;
  }
  .grass {
    background: #DEFDE0, !important;
  }
  .electric {
    background: #FCF7DE, !important;
  }
  .water {
    background: #DEF3FD, !important;
  }
  .ground {
    background: #f4e4da, !important;
  }
  .rock {
    background: #d5d5d4, !important;
  }
  .fairy {
    background: #fceaff, !important;
  }
  .poison {
    background: #98d7a5, !important;
  }
  .bug {
    background: #f8d5a5, !important;
  }
  .dragon {
    background: #97b3e6, !important;
  }
  .psychic {
    background: #eaeda1, !important;
  }
  .flying {
    background: #F5F5F5, !important;
  }
  .fighting {
    background: #E6E0D4, !important;
  }
  .normal {
    background: #F5F5F5 !important;
  }
}
i.fa-spinner {
  text-align: center;
}
.detail {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  position: fixed;
  top: 0;
  left: 0;
  padding: 90px 10px 10px;
  //width: calc(100% - 20px);
  // height: calc(100vh - 20px);
  width: 100%;
  height: 100vh;
  background: rgba(10, 7, 0, 0.562)
}
.detail-view {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  width: 90%;
  padding: 50px 0 0;
  position: relative;
  max-width: 510px;
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 15px 30px rgba(0, 0, 0, .2),
    0 10px 10px rgba(0, 0, 0, .2);
}
.image {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: -60px;
  width: 120px;
  height: 120px;
  background-color: #ffcb04;
  border-radius: 50%;
  overflow: hidden;
  box-shadow: 0 15px 30px rgba(0, 0, 0, .2),
    0 10px 10px rgba(0, 0, 0, .2);
}
h2 {
  text-transform: capitalize;
}
.data {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-direction: column;
  width: 100%;
  margin-bottom: 40px;
}
.property {
  width: 90%;
  max-width: 400px;
  border-bottom: 1px solid #ccc;
  margin-bottom: 10px;
}
.left {
  float: left;
}
.right {
  float: right;
}
h3 {
  width: 90%;
  max-width: 400px;
  border-bottom: 1px solid #ccc;
}
.types,
.abilities {
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;
  width: 90%;
  max-width: 400px;
}

.type {
  // color: rgb(17, 67, 182);
  margin: 0 0 10px 0;
  padding: 5px 10px;
  font-weight: bold;
  font-size: 1rem;
  letter-spacing: 2px;
  text-transform: capitalize;

  span {
    color: #C73015 !important;
    padding: 10px 14px;
    border-radius: 29px;
  }
}

.ability {
  color: rgb(10, 119, 10);
  margin: 0 10px 10px 0;
  border-radius: 20px;
  padding: 5px 10px;
  font-weight: bold;
  font-size: 1rem;
  letter-spacing: 2px;
  text-transform: capitalize;
  word-wrap: none;
  word-break: keep-all;
  background-color: #ffffff;
  border: 3px solid;
}
.close {
  outline: none;
  border: none;
  border-radius: 5px;
  background-color: #C73015;
  color: #efefef;
  padding: 10px 20px;
  margin-bottom: 20px;
  font-size: 1.2rem;
  cursor: pointer;
}
i {
  font-size: 2rem;
  color: #efefef;
}
.destaq {
  font-weight: bold;
}
.inactive {
  display: none;
}
</style>