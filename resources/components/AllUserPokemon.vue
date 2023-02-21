<template>
  <div class="container">
    <div class="column welcome-div">
      <img src="../assets/img/pokemon_logo.png" width="250" height="100" />
      <p v-if="users.length > 0" style="margin-top: 5px">List of all Users</p>
    </div>
    <div v-if="users.length > 0" class="divPokemon">
      <div
        class="card border-primary perPokemon"
        v-for="(item, ndx) in fetchUsers"
        :key="ndx"
        @click="showModal(item)"
      >
        <div class="user-div">
          <div style="text-align: center">
            <img
              src="../assets/img/profile-user.png"
              width="100"
              height="100"
              :alt="item"
            />
          </div>
          <div class="card-body">
            <h5 class="card-title"> {{ item.name }} </h5>
          </div>
        </div>
      </div>

      <!-- <button
        type="button"
        class="btn btn-secondary"
        v-if="offset > 0"
        @click="fetchAllPokemon(false)"
      >
        Previous
      </button>
      <button
        type="button"
        class="btn btn-primary"
        v-if="offset < count + 12"
        @click="fetchAllPokemon(true)"
      >
        Next
      </button> -->
    </div>
    <div v-else class="column welcome-div emptyUser">
      <p>No</p><br>
      <p>Other User</p>
    </div>
    <UserPokemon ref="userPokemon" @onConfirm="remove($event)" />
  </div>
  <div style="height: 50px;"></div>
</template>
<script>
import UserPokemon from "./UserPokemon.vue";
import AUTH from "../services/auth";
export default {
  mounted() {
    this.fetchAllUser();
  },
  data() {
    return {
      users: [],
      pokemon_information: null,
      selected: null,
      offset: 0,
      limit: 12,
      count: 0,
      id: null,
      search: null
    };
  },
  computed: {
    fetchUsers() {
      return this.users.filter((item) => {
        return item.name;
      });
    },
  },
  methods: {
    fetchAllUser(flag) {
      if (flag != null) {
        if (flag) {
          this.offset += this.limit;
        } else {
          this.offset -= this.limit;
        }
      }
      // fetch(
      //   `https://pokeapi.co/api/v2/pokemon?limit=${this.limit}&offset=${this.offset}`
      // )
      //   .then((response) => response.json())
      //   .then((allpokemon) => {
      //     this.users = allpokemon.results;
      //     this.count = allpokemon.count;
      //   });
      let params = {
        id: AUTH.userId
      };
      this.$axios.post(AUTH.url + 'retrievePokemon', params, AUTH.config)
      .then(response => {
        this.users = response.data
      })
      .catch(e => {
        if(e.response && e.response.status === 401){
          AUTH.deauthenticate()
        }
      });
    },
    showModal(item) {
      this.$refs.userPokemon.show(item);
    },
  },
  components: {
    UserPokemon,
  },
};
</script>
<style scoped lang="scss">
// .user-div{
//   padding: 20px;
// }
.divPokemon {
  display: flex;
  flex-wrap: wrap;
}

.welcome-div {
  text-align: center;
  margin-top: 5%;
}

.perPokemon {
  width: 23%;
  margin: 1%;
  text-align: center;
  height: 170px;
  max-width: 100%;
  padding: 15px;
}

.card {
  border-radius: 25px;
  background: #fff;
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.08), 0 0 6px rgba(0, 0, 0, 0.05);
  transition: 0.3s transform cubic-bezier(0.155, 1.105, 0.295, 1.12),
    0.3s box-shadow,
    0.3s -webkit-transform cubic-bezier(0.155, 1.105, 0.295, 1.12);
  cursor: pointer;
}

.card:hover {
  transform: scale(1.05);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.12), 0 4px 8px rgba(0, 0, 0, 0.06);
}

.btn-secondary {
  margin-right: 1%;
}
.emptyUser {
  text-align: center;
  font-family:serif;
  letter-spacing: 1em;
  width: 80%;
  margin-bottom: 15px;
  margin-left: auto;
  margin-right: auto;
  padding: 2%;
  height: 10%;
  background: #fff;
  border-radius: 25px;
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.08), 0 0 6px rgba(0, 0, 0, 0.05);
  transition: 0.3s transform cubic-bezier(0.155, 1.105, 0.295, 1.12),
    0.3s box-shadow,
    0.3s -webkit-transform cubic-bezier(0.155, 1.105, 0.295, 1.12);
}
.emptyUser p {
  font-size: 50px;
}
</style>
