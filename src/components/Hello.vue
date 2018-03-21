<template>
  <div>
    <ul class="breed-list"> <!-- v-if="posts && posts.length" -->
      <li v-for="(post, name, index) of posts" v-bind:key="index">
        <p @click="getBreedInfo(name)">{{ name }}</p>
        <p @click="post[0].toggle = !post[0].toggle"  v-if="post.length > 1" class="seemore">Click to see sub breeds</p>
        <ul v-if="(post.length > 1) && post[0].toggle">
          <li v-for="(subbreed, name) of post" v-bind:key="name" v-if="name">
            {{ subbreed }}
          </li>
        </ul>
      </li>
    </ul>
    <div class="breed-info">
      <img :src="selectedBreed.img">
    </div>

    <!-- <ul v-if="errors && errors.length">
      <li v-for="error of errors">
        {{error.message}}
      </li>
    </ul> -->
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'hello',
  data () {
    return {
      posts: [],
      errors: [],
      breedTypesVis: [],
      selectedBreed: {
        img: 'https://placehold.it/300x300'
      }
    }
  },
  created() {
    axios.get(`https://dog.ceo/api/breeds/list/all`)
    .then(response => {
      let breeds = response.data.message;
      var step;
      for (step = 0; step < Object.keys(breeds).length; step++) {
        let name = Object.keys(breeds)[step];
        breeds[name].unshift({"toggle": false});
      }
      this.posts = breeds
    })
    .catch(e => {
      this.errors.push(e)
      console.log(e)
    })
  },
  methods: {
     getBreedInfo: function(name) {
      axios.get(`https://dog.ceo/api/breed/${name}/images/random`)
      .then(response => {        
        this.selectedBreed.img = response.data.message
      })
      .catch(e => {
        console.log(e)
      })
     }
  }
}
</script>

<style scoped>
.breed-info {
  position: fixed;
  width: 20%;
  right: 0;
}
.breed-info img{
  width: 100%;
}
.breed-list {
  width: 80%;
  float: left;
  text-transform: capitalize;
}
.breed-list li {
  width: 20%;
  background-color: #ccc;
  padding: 7px;
  margin-top: 5px;
}
/* SUBLISTS */
.breed-list li .seemore {
  cursor: pointer;
}
.breed-list ul li {
  width: auto;
  background-color: #fbffbf;
  padding: 0;
  margin-top: 0;
}
h1, h2 {
  font-weight: normal;
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
  color: #35495E;
}
</style>
