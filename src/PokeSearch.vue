<template>
    <div class="searchbar">
        <form @submit.prevent="setPokemonUrl">
            <input type="text" placeholder="Consulta un pokemon por su ID o su nombre" v-model="searchvalue">
            <p class="legenda">Ingresa número ID o nombre del pokemon</p>
        </form>
        <i class="fas fa-search" @click="setPokemonUrl"></i>
    </div>
</template>
  
<script>
export default {
    data: () => {
        return {
            searchvalue: '',
            apiUrl: "https://pokeapi.co/api/v2/pokemon/",
        }
    },
    methods: {
        setPokemonUrl(url) {
            if (this.searchvalue !== '')
                this.$emit('setPokemonUrl', this.apiUrl + this.searchvalue);
        }
    }
}
</script>
  
<style lang="scss" scoped>
@import url("https://use.fontawesome.com/releases/v5.8.1/css/all.css");

.searchbar {
    position: relative;
    width: 100%;
    max-width: 510px;
    padding-bottom: 20px;
}

input {
    border: none;
    outline: none;
    border-radius: 5px;
    padding: 10px 40px 10px 10px;
    width: calc(100% - 50px);
    font-size: 1rem;
    box-shadow: 0 15px 30px rgba(0, 0, 0, .2),
        0 10px 10px rgba(0, 0, 0, .2);
}
.legenda {
    color: #fff;
}
i {
    position: absolute;
    top: 10px;
    right: 10px;
    font-size: 1.25rem;
    color: #0A2E50;
    cursor: pointer;
}
</style>
  