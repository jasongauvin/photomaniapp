<template>
  <div>
    <nav class="navbar">
      <button type="submit" class="btn btn--green btn--go" @click.prevent="search">Go</button>
    </nav>
    <div class="wrapper">
      <p v-if="loading" class="text-centered">Loading...</p>
      <ul v-else class="image-card-grid">
        <image-card v-for="image in cleanImages" :key="image.id" :image="image" />
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ImageCard from "@/components/ImageCard";
export default {
  name: "home",
  components: {
    ImageCard
  },
  data() {
    return {
      loading: false,
      images: []
    };
  },
  computed: {
    cleanImages() {
      return this.images.filter(image => image.url);
    }
  },
  methods: {
    search() {
      this.loading = true;
      this.fetchImages()
        .then(response => {
          console.log(response);
          this.images = response.data;
          this.loading = false;
        })
        .catch(error => {
          console.log("An error ocurred: ", error);
        });
    },
    fetchImages() {
      return axios({
        method: "get",
        url: "https://picsum.photos/v2/list?page=2&limit=30",
        params: {
          extras: "url, author, id, download_url",
          page: 1,
          format: "json",
          nojsoncallback: 1,
          per_page: 30
        }
      });
    }
  }
};
</script>

<style lang="scss">
.screen-reader-only {
  height: 1px;
  width: 1px;
  position: absolute;
  left: -100000px;
}
.text-centered {
  text-align: center;
}
.wrapper {
  margin: 0 auto;
  max-width: 800px;
  @media only screen and (max-width: 799px) {
    max-width: 100%;
    margin: 0 1.5rem;
  }
}
.image-card-grid {
  list-style: none;
  margin: 0.5rem 0;
  padding: 0;
  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;
}
.navbar {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  background: #f0f0f0;
}
.searchbar {
  width: 300px;
  display: flex;
  align-items: center;
  justify-content: center;
  @media only screen and (max-width: 549px) {
    width: 100%;
    label {
      width: 80%;
    }
  }
}
.searchbar-input {
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-size: 1rem;
  border: 1px solid gray;
  min-width: 300px;
  @media only screen and (max-width: 549px) {
    min-width: 0;
    width: 100%;
  }
}
.btn {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  border-radius: 20px;
  background: transparent;
  border: none;
}
.btn--green {
  background: #42b983;
  color: white;
  font-weight: bold;
}
.btn--go {
  padding: 0.5rem 2rem;
  margin-left: 1rem;
}
</style>