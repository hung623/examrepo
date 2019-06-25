<template>
  <div>
    <p>RG lunch list</p>
    <li v-for="people in peoples, sortedPeople" :key="people.id">
    {{ people.name }}
    {{ formatString(people.counter) }}
    <button v-if="people.edit = true" @click=" counter += 1">Made lunch!</button>
    </li>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "app",
  data: function() {
    return {
      peoples: [],
      counter: 0,
      failure: ""
    };
  },
  computed: {
    sortedPeople: function() {
      return this.counter.sort();
    }
  },
  methods: {
    getPeople: function() {
      let self = this;
      axios
        .get("https://api.jsonbin.io/b/5d11fe72c8516d4ebcbb3b16/1")
        .then(function(response) {
          // handle success
          console.log(response);
          self.peoples = response.data;
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
        .put("https://api.jsonbin.io/b/5d11fe72c8516d4ebcbb3b16/1", self.peoples)
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

<style>

</style>
