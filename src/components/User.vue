<template>
  <div>
    <h1>VueJS example</h1>
    <hr />
    <button id="fetch-users" @click="fetchUsers">
      Fetch users
    </button>

    <label>
      <input v-model="genderFilter" type="checkbox" value="male" />
      Hommes
    </label>
    <label>
      <input v-model="genderFilter" type="checkbox" value="female" />
      Femmes
    </label>
    <div id="none">
      {{ genderFilter }}
    </div>
    <div>
      <label>
        <input v-model="filterByName" placeholder="Rechercher un utilisateur" />
      </label>
    </div>
    <table id="tbl-users" class="table table-hover">
      <thead>
        <tr>
          <th />
          <th>Nom</th>
          <th>Email</th>
          <th>Tel</th>
          <th>Genre</th>
          <th>
            Age
            <svg
              data-v-1061b08a=""
              width="16"
              height="16"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 320 512"
            >
              <path
                data-v-1061b08a=""
                fill="currentColor"
                d="M41 288h238c21.4 0 32.1 25.9 17 41L177 448c-9.4 9.4-24.6 9.4-33.9 0L24 329c-15.1-15.1-4.4-41 17-41zm255-105L177 64c-9.4-9.4-24.6-9.4-33.9 0L24 183c-15.1 15.1-4.4 41 17 41h238c21.4 0 32.1-25.9 17-41z"
              />
            </svg>
          </th>
        </tr>
      </thead>
      <tbody id="tbody-users">
        <tr v-for="user in namesFiltered" :key="user.age">
          <td><img :src="user.picture.thumbnail" /></td>
          <td>{{ user.name.first }} {{ user.name.last }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.phone }}</td>
          <td>{{ user.gender }}</td>
          <td>{{ user.dob.age }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "User",
  data() {
    return {
      results: [],
      errored: false,
      genderFilter: ["male", "female"],
      filterByName: ""
    };
  },
  computed: {
    usersFiltered() {
      return this.results.filter(user =>
        this.genderFilter.includes(user.gender)
      );
    },
    namesFiltered() {
      return this.usersFiltered.filter(user => {
        return (user.name.first + " " + user.name.last)
          .toLowerCase()
          .includes(this.filterByName.toLowerCase());
      });
    }
  },
  methods: {
    fetchUsers() {
      axios
        .get("https://randomuser.me/api/?results=20")
        .then(response => (this.results = response.data.results))
        .catch(error => {
          console.log(error);
          this.errored = true;
        });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  margin: 20px 0 0;
  color: purple;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
#none{
  display:none;
}
</style>
