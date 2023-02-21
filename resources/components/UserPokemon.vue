<template>
  <div
    class="modal fade"
    id="userModal"
    tabindex="-1"
    role="dialog"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modalTop">
          <div class="nav nav-tabs" id="nav-tab" role="tablist">
            <button @click="hateLikeFave('favorite')" class="modal-title text-primary nav-link active" id="user-favorite" data-bs-toggle="tab" data-bs-target="#nav-favorite" type="button" role="tab" aria-controls="nav-favorite" aria-selected="true">Favorite</button>
            <button @click="hateLikeFave('like')" class="modal-title text-primary nav-link" id="user-like" data-bs-toggle="tab" data-bs-target="#nav-like" type="button" role="tab" aria-controls="nav-like" aria-selected="false">Like</button>
            <button @click="hateLikeFave('hate')" class="modal-title text-primary nav-link" id="user-hate" data-bs-toggle="tab" data-bs-target="#nav-hate" type="button" role="tab" aria-controls="nav-hate" aria-selected="false">Hate</button>
          </div>
          <button
            type="button"
            class="close"
            data-dismiss="modal"
            aria-label="Close"
            @click="hideModal()"
          >
            <span aria-hidden="true" class="text-primary">&times;</span>
          </button>
        </div>
        <div class="tab-content" id="nav-tabContent">
          <div class="tab-pane fade show active" id="nav-favorite" role="tabpanel" aria-labelledby="user-favorite">
            <div v-if="faveName != null" class="container">
              <div class="divPokemon">
                <div class="container1 border-primary perPokemon">
                  <div style="text-align: center">
                    <img
                      :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/home/${faveId}.png`"
                      width="150"
                      height="150"
                      :alt="faveName"
                    />
                    <p>
                      <b>{{ helper.displayWithCamelCase(faveName ? faveName : '') }}</b>
                    </p>
                  </div>
                  <div class="row" style="margin-left: 10%; margin-right: 10%">
                    <div>
                      <p><b>Type:</b> {{ helper.displayWithCamelCase(faveType ? faveType : '') }}</p>
                      <b>Abilities:</b>
                      <div v-for="(itm, ndx) in faveAbilities" :key="ndx">
                        <ul>
                          <li>{{ helper.displayWithCamelCase(itm) }}</li>
                        </ul>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div v-else class="border-primary emptyPokemon">
              <div class="column welcome-div emptyFont">
                <img src="../assets/img/pokemon_logo.png" width="250" height="100" /><br><br>
                <h3>No</h3><br>
                <h3>Favorite</h3>
              </div>
            </div>
          </div>
          <div class="tab-pane fade" id="nav-like" role="tabpanel" aria-labelledby="user-like">
            <div v-if="pokemons.length > 0" class="divPokemon">
              <div
                class="container1 border-primary likePokemon"
                v-for="(item, ndx) in pokemons"
                :key="ndx"
              >
                <div style="text-align: center">
                  <img
                    :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/home/${item.id}.png`"
                    width="150"
                    height="150"
                    :alt="item.name"
                  />
                  <p>
                    <b>{{ helper.displayWithCamelCase(item.name ? item.name : '') }}</b>
                  </p>
                </div>
                <div class="row" style="margin-left: 10%; margin-right: 10%">
                  <p><b>Type:</b> {{ item.type }}</p>
                  <b>Abilities:</b>
                  <div v-for="(itm, ndx) in item.abilities" :key="ndx">
                    <ul>
                      <li>{{ helper.displayWithCamelCase(itm) }}</li>
                    </ul>
                  </div>
                  <!-- <p><b>Description:</b> {{ item.details }}</p> -->
                </div>
              </div>
            </div>
            <div v-else class="border-primary emptyPokemon">
              <div class="column welcome-div emptyFont">
                <img src="../assets/img/pokemon_logo.png" width="250" height="100" /><br><br>
                <h3>No</h3><br>
                <h3>Liked</h3>
              </div>
            </div>
          </div>
          <div class="tab-pane fade" id="nav-hate" role="tabpanel" aria-labelledby="user-hate">
            <div v-if="pokemons.length > 0" class="divPokemon">
              <div
                class="container1 border-primary likePokemon"
                v-for="(item, ndx) in pokemons"
                :key="ndx"
              >
                <div style="text-align: center">
                  <img
                    :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/home/${item.id}.png`"
                    width="150"
                    height="150"
                    :alt="item.name"
                  />
                  <p>
                    <b>{{ helper.displayWithCamelCase(item.name ? item.name : '') }}</b>
                  </p>
                </div>
                <div class="row" style="margin-left: 10%; margin-right: 10%">
                  <p><b>Type:</b> {{ item.type }}</p>
                  <b>Abilities:</b>
                  <div v-for="(itm, ndx) in item.abilities" :key="ndx">
                    <ul>
                      <li>{{ helper.displayWithCamelCase(itm) }}</li>
                    </ul>
                  </div>
                  <!-- <p><b>Description:</b> {{ item.details }}</p> -->
                </div>
              </div>
            </div>
            <div v-else class="border-primary emptyPokemon">
              <div class="column welcome-div emptyFont">
                <img src="../assets/img/pokemon_logo.png" width="250" height="100" /><br><br>
                <h3>No</h3><br>
                <h3>Hated</h3>
              </div>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-danger"
            data-toggle="tooltip"
            data-placement="top"
            title="Add to hate"
            @click="addHateLikeFav('hate')"
          >
            <font-awesome-icon icon="fa-solid fa-thumbs-down" />
          </button>
          <button
            type="button"
            class="btn btn-primary"
            data-toggle="tooltip"
            data-placement="top"
            title="Add to like"
            @click="addHateLikeFav('like')"
          >
            <font-awesome-icon icon="fa-solid fa-thumbs-up" />
          </button>
          <button
            type="button"
            class="btn btn-warning"
            data-toggle="tooltip"
            data-placement="top"
            title="Add to favorite"
            @click="addHateLikeFav('fave')"
          >
            <font-awesome-icon icon="fa-solid fa-heart" />
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Helper from '../services/helper';
import $ from "jquery";
import AUTH from "../services/auth";
$(function () {
  $('[data-toggle="tooltip"]').tooltip();
});
export default {
  mounted() {},
  data() {
    return {
			helper: Helper,
      faveName: null,
      faveId: null,
      faveAbilities: [],
      faveType: null,
      pokemons: [],
      user: null
    };
  },
  methods: {
    get_id(pokemon) {
      return Number(pokemon.url.split("/")[6]);
    },
    get_name(pokemon) {
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
    },
    show(item) {
      this.user = item;
      this.faveName = null,
      this.faveId = null,
      this.faveAbilities = [],
      this.faveType = null,
      $( "#user-favorite" ).trigger( "click" );
      $("#userModal").modal("show")
      this.fetchSpecificPokemon(item.favorite, 'favorite');
    },
    hideModal() {
      this.user = null;
      $("#userModal").modal("hide");
    },
    hateLikeFave(param){
      if(param === 'favorite'){
        if(this.user.favorite){
          this.fetchSpecificPokemon(this.user.favorite, param);
        }
      }
      if(param === 'like'){
        if(this.user.like.length > 0){
          this.user.like.forEach(el => {
            this.fetchSpecificPokemon(el, param);
          })
        }
      }
      if(param === 'hate'){
        if(this.user.hate.length > 0){
          this.user.hate.forEach(el => {
            this.fetchSpecificPokemon(el, param);
          })
        }
      }
    },
    fetchSpecificPokemon(name, param) {
      this.pokemons = []
      fetch(`https://pokeapi.co/api/v2/pokemon/${name}/`)
        .then((response) => response.json())
        .then((details) => {
          let abilities = [];
          let type = details.types[0].type.name.charAt(0).toUpperCase() + details.types[0].type.name.slice(1);
          details.abilities.forEach((ability) => {
            abilities = [...abilities, ability.ability.name];
          });
          if(param === 'favorite'){
            this.faveId = details.id
            this.faveName = name
            this.faveType = type
            this.faveAbilities = abilities
          }else{
            this.pokemons.push({'id': details.id, 'name': name, 'abilities': abilities, 'type': type})
          }
        });
    }
  },
};
</script>
<style scoped>
.card-title {
  text-align: center;
}
.modalTop{
  display: flex;
  flex-shrink: 0;
  align-items: center;
  justify-content: space-between;
  padding: var(--bs-modal-header-padding);
  /* border-bottom: var(--bs-modal-header-border-width) solid var(--bs-modal-header-border-color); */
  border-top-left-radius: var(--bs-modal-inner-border-radius);
  border-top-right-radius: var(--bs-modal-inner-border-radius);
}
#nav-tab{
  width: 100%;
}
.container{
  padding: 25px;
}
.divPokemon {
  display: flex;
  flex-wrap: wrap;
}
.welcome-div {
  text-align: center;
  margin-top: 5%;
}
.perPokemon {
  width: 98%;
  margin: 1%;
  padding: 2%;
  height: 10%;
}
.likePokemon {
  width: 80%;
  margin-bottom: 15px;
  margin-left: auto;
  margin-right: auto;
  padding: 2%;
  height: 10%;
}
.emptyFont {
  font-family:serif;
  letter-spacing: 1em;
}
.emptyPokemon {
  width: 80%;
  margin-bottom: 15px;
  margin-left: auto;
  margin-right: auto;
  padding: 2%;
  height: 10%;
  background-image: linear-gradient(red, yellow);
  border-radius: 25px;
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.08), 0 0 6px rgba(0, 0, 0, 0.05);
  transition: 0.3s transform cubic-bezier(0.155, 1.105, 0.295, 1.12),
    0.3s box-shadow,
    0.3s -webkit-transform cubic-bezier(0.155, 1.105, 0.295, 1.12);
}
.container1 {
  border-radius: 25px;
  background: #fff;
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.08), 0 0 6px rgba(0, 0, 0, 0.05);
  transition: 0.3s transform cubic-bezier(0.155, 1.105, 0.295, 1.12),
    0.3s box-shadow,
    0.3s -webkit-transform cubic-bezier(0.155, 1.105, 0.295, 1.12);
  /* cursor: pointer; */
}
.container1:hover {
  transform: scale(1.05);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.12), 0 4px 8px rgba(0, 0, 0, 0.06);
}
.btn-secondary {
  margin-right: 1%;
}
</style>