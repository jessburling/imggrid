<template>
  <div id="library" class="library">
      <div class="library-item" v-for="photo in photos" v-on:click="getPhotoId(photo)">
        <div id="check">
          <i class="fas fa-check-circle" v-bind:class="setClass(photo)"></i>
        </div>
      <img class="library-image" v-bind:src="photo.url" />
  </div>
  </div>
</template>

<script>

import axios from 'axios';

export default {
  name: "Home",
  data() {
    return {
      photos: [],
      // imgSelected: false,
      selectedList: [],
    }
  },

created() {
    this.fetchData()
  },

  methods: {
    fetchData() {
      axios.get('https://jsonplaceholder.typicode.com/photos?albumId=1')
        .then(response => this.buildPhotoList(response.data))
          .catch((err) => {
            console.log(err);
            })
        .finally(() => this.loading = false)
    },
    buildPhotoList(data) {
      this.photos = data;
    },
    // selectImg() {
    //   this.imgSelected = !this.imgSelected
    //   console.log(this.imgSelected)
    // },
    getPhotoId(photo) {
      if (this.selectedList.includes(photo.id)) {
        this.selectedList = this.selectedList.filter((e) => e !== photo )
        console.log(`Removed` + photo.id)
      } else {
      this.selectedList.push(photo.id)
      }
    },
    setClass(photo) {
      if (this.selectedList.includes(photo.id)) {
        return "fa-check-circle true"
    } else {
        return "fa-check-circle false"
    }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .library {
    display: flex;
    flex-wrap: wrap;
    margin: 0 auto;
    max-width: 70rem;
    padding-left: 7rem;
  }

  .library-item {
    position: relative;
    padding: 0.5rem;
  }

  .library-image {
    height: 12rem;
    width: 12rem;
    cursor: pointer;
  }

  .item-name {
    position: absolute;
    width: 20rem;
    background-color: #01b3bd;
    color: white;
  }

  #check {
    position: absolute;
    width: 11rem;
    text-align: right;
    padding-top: 1rem;
  }

  .fa-check-circle, .true {
    color: white;
  }

  .false {
    display: none;
  }

  @media only screen and (max-width: 500px) {
    

    .library {
      max-width: 28rem;
      margin: auto;
      padding-left: 0rem;
    }
  }
</style>
