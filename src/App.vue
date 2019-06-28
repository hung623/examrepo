<template>
  <div>
    <div>
      <p>RG lunch list</p>
    </div>
    <div>
      <input type="form" v-model="newPerson">
      <button type="button" @click="addNewPerson(newPerson)">submit new coworker </button>
      <button @click="deleteUser = !deleteUser">Toggle between delete/made lunsj buttons</button>
    </div>
    <ul style="list-style: none;">
      <li v-for="(person, id) in people" :key="person.id">
      <p>{{ person.name }} times made lunch {{ person.counter }}
      <button v-if="deleteUser" @click="updateData(person.counter++)">Made lunch!</button>
      <button v-if="!deleteUser" @click="remove(id)">Delete a user</button>
      </p>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "app",
  data: function() {
    return {
      people: [],
      newPerson: "",
      deleteUser: true,
    };
  },
  methods: {
    remove: function(personId) {
      this.people.splice(personId, 1);
      this.updateData();
    },
    addNewPerson: function(newPerson) {
      this.people.push({ name: newPerson, counter: 0,});
      this.newPerson = "";
      console.log(this.people);
      this.updateData();
    },
    getPeople: function() {
      let self = this;
      axios
        .get("//api.jsonbin.io/b/5d11fe72c8516d4ebcbb3b16/latest")
        .then(function(response) {
          // handle success
          console.log(response);
          self.people = response.data;
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        })
        .then(function() {
          // always executed
        });
     },
     updateData: function() {
      let self = this;
      axios
        .put("//api.jsonbin.io/b/5d11fe72c8516d4ebcbb3b16", self.people)
        .then(function(response) {
          // handle success
          console.log(response);
          console.log("it works!");
        })
        .catch(function(error) {
          // handle error
          console.log(error);
        })
        .then(function() {
          // always executed
        });
    },
    formatString: function() {
      return parseInt(this.counter)
    },
  },   
  beforeMount() {
    this.getPeople();
    // this runs before the page renders and will grab people from json list
  }
}
</script>
