<template>
  <div>
    <h1>VueJS example</h1>
    <hr />
    <button id="fetch-users" @click="fetchUsers">
      Fetch users
    </button>
    <input v-model="genderFilter" type="checkbox" value="male" />
    Hommes
    <input v-model="genderFilter" type="checkbox" value="female" />
    Femmes
    <div id="none">
      {{ genderFilter }}
    </div>
    <div>
      <input v-model="filterByName" placeholder="Rechercher un utilisateur" />
    </div>
    <table id="tbl-users" class="table table-hover">
      <thead>
        <tr>
          <th />
          <th>Nom</th>
          <th>Email</th>
          <th>Tel</th>
          <th>Genre</th>
          <th @click="sort('age')">
            <div v-if="(currentSortDir = 'desc')">
              Age
              <svg
                aria-hidden="true"
                height="14"
                width="14"
                focusable="false"
                data-prefix="fas"
                data-icon="sort-up"
                class="svg-inline--fa fa-sort-up fa-w-10"
                role="img"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 320 512"
              >
                <path
                  fill="currentColor"
                  d="M279 224H41c-21.4 0-32.1-25.9-17-41L143 64c9.4-9.4 24.6-9.4 33.9 0l119 119c15.2 15.1 4.5 41-16.9 41z"
                ></path>
              </svg>
            </div>
            <div v-else>
              <svg
                aria-hidden="true"
                height="14"
                width="14"
                focusable="false"
                data-prefix="fas"
                data-icon="sort-down"
                class="svg-inline--fa fa-sort-down fa-w-10"
                role="img"
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 320 512"
              >
                <path
                  fill="currentColor"
                  d="M41 288h238c21.4 0 32.1 25.9 17 41L177 448c-9.4 9.4-24.6 9.4-33.9 0L24 329c-15.1-15.1-4.4-41 17-41z"
                ></path>
              </svg>
            </div>
          </th>
        </tr>
      </thead>
      <tbody id="tbody-users">
        <tr v-for="user in namesFiltered" :key="user.name">
          <td><img :src="user.picture.thumbnail" /></td>
          <td>{{ user.name.first }} {{ user.name.last }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.phone }}</td>
          <td>{{ user.gender }}</td>
          <td>{{ user.dob.age }}</td>
        </tr>
      </tbody>
    </table>

    <div id="none2">
      debug: sort={{ currentSort }}, dir={{ currentSortDir }}
    </div>
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
      filterByName: "",
      currentSort: "age",
      currentSortDir: "asc"
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
    },
    agesFiltered() {
      return this.namesFiltered.slice().sort((a, b) => {
        let modifier = 1;
        if (this.currentSortDir === "desc") modifier = -1;
        if (a.dob.age < b.dob.age) return -1 * modifier;
        if (a.dob.age > b.dob.age) return modifier;
        return 0;
      });
    }
  },
  methods: {
    fetchUsers() {
      axios
        .get("https://randomuser.me/api/?results=20")
        .then(response => (this.results.push.apply(this.results, response.data.results))
        .catch(error => {
          console.log(error);
          this.errored = true;
        }));
    },
    sort(s) {
      
      if (s === this.currentSort) {
        this.currentSortDir = this.currentSortDir === "asc" ? "desc" : "asc";
      }
      this.currentSort = s;
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
#none {
  display: none;
}
#none2 {
  display: none;
}
</style>
