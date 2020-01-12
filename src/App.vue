<template>
  <div id="app">
    <div class="nav" >
    <div class="cont">  
    <input type="text" placeholder="Enter search " v-model='searchitem' @keydown.enter.prevent="search"/>
    <button class="btn btn-success" @click="search">Search</button>
    </div>
    </div>
    <div v-if="isLoading" class="spinner-border sp" style="width: 3rem; height: 3rem;" role="status">
  <span class="sr-only">Loading...</span>
</div>
    <ul v-if="!isLoading" class="cards">
          <searchItems v-for='result in image_results' :source_url="result.url" :key="result" :title='result.title' :image_url='result.image_url'/>
    </ul>
    <observer v-if='seeing' v-on:intersect='intersect'/>
  </div>
</template>

<script>
import searchItems from './components/searchItems'
import observer from './components/observer'
import axios from 'axios'
 

export default {
  name: 'app',
  data() {
    return {
      isLoading:false,
      searchitem:'',
      image_results: {},
      prev:20,
      next:39,
      seeing:false,
      access_key:'89ab391f385e61296c9cd695c65ec5fe',
      page:1
    }
  },
  computed: {
    filter_image:function () {
      return this.image_results.filter(result => result.position<=this.prev)
    },
  },
  components: {
    searchItems,
    observer
  },
  methods : {
    search() {
      this.isLoading=true;
      axios.get(`http://api.serpstack.com/search?access_key=${this.access_key}&query=${this.searchitem}&type=images&images_page=1`)
      .then( res => {
        this.image_results = res.data.image_results.filter(result => result.position<=this.prev);
        this.isLoading=false;
        this.seeing=true;
      })
    },
    overe(a) {
      this.searchitem=a;
    },
    intersect() {
      
      axios.get(`http://api.serpstack.com/search?access_key=${this.access_key}&query=${this.searchitem}&type=images&images_page=${this.page}`)
      .then( res => {
        this.nextres = res.data.image_results.filter(result => result.position<=this.next+1 && result.position>=this.prev+1);
        this.image_results = [...this.image_results,...this.nextres]
        if (this.next % 79 == 0) {
        this.page++
        this.prev=20
        this.next=39
        }else {
        this.prev+=20
        this.next+=20
        }
      })
      }
    
  }
}
</script>

<style lang="scss">
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin-top :10px;
  width:100%;
}
.sp {
  position:absolute;
  display: flex;
  justify-content: center;
  align-items:center;
  top:40%;
}
.container1 {
  height:85vh;
  width: 100%;
  margin-top: 10px;
  padding:0;
}

input {
  height:2rem;
  width: 15rem;
  padding: 10px;
  font-size: 1rem;
}

/* Font */
@import url('https://fonts.googleapis.com/css?family=Quicksand:400,700');

/* Design */
*,
*::before,
*::after {
  box-sizing: border-box;
}

html {
  background-color: #ecf9ff;
}

body {
  color: #272727;
  font-family: 'Quicksand', serif;
  font-style: normal;
  font-weight: 400;
  letter-spacing: 0;
  padding: 1rem;
}
.card_image {
  display: flex;
  justify-content: center;
}
h1 {
    font-size: 24px;
    font-weight: 400;
    text-align: center;
}

img {
  height: 300px;
  max-width: 100%;
  vertical-align: middle;

}


.cards {
  display: flex;
  flex-wrap: wrap;
  list-style: none;
  margin: 0;
  padding: 0;
}

.cards_item {
  display: flex;
  padding: 0.5rem;
  width: 100%;
}

@media (min-width: 40rem) {
  .cards_item {
    width: 50%;
  }
}

@media (min-width: 56rem) {
  .cards_item {
    width: 25%;
  } 
}

.card {
  background-color: white;
  border-radius: 0.25rem;
  box-shadow: 0 20px 40px -14px rgba(0, 0, 0, 0.25);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  width:100%;
}

.card_content {
  padding: 1rem;
  height: 100%;
  background: linear-gradient(to bottom left, #EF8D9C 40%, #FFC39E 100%);
}

.card_title {
  color: #ffffff;
  font-size: 1.1rem;
  font-weight: 700;
  letter-spacing: 1px;
  text-transform: capitalize;
  margin: 0px;
}

.card_text {
  color: #ffffff;
  font-size: 0.875rem;
  line-height: 1.5;
  margin-bottom: 1.25rem;    
  font-weight: 400;
}
</style>
