<template>
      <li class="cards_item" :class="{ remov: rem }">
      <div class="card">
        <div class="card_image"><a :href='image_url'><img @error="onImageLoadFailure($event)" :src='image_url' alt="NOP"></a></div>
        <div class="card_content">
          <a :href="source_url" ><h2 class="card_title">{{ title }}</h2></a>
        </div>
      </div>
    </li>
</template>

<script>
import $ from 'jquery'

export default {
  name: 'searchItems',
  data() {
    return {
      rem :false
    }
  },
  props: ['image_url','title',"source_url"],
  methods : {
    onImageLoadFailure (event) {
        this.rem=true;
        event.target.src = 'https://www.jqueryscript.net/images/loading-indicator-view.jpg'     
        $('.remov').remove();
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
a {
  text-decoration: none;
}
.card:hover {
  transform: translateY(-10px);
  animation: all 2s ease;
}

</style>
