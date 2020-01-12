<template>
    <div class="observer">
        <h2>See More</h2>
        {{ valu}}
    </div>
</template>

<script>
export default {
  name: 'observer',
  data() {
    return {
      observer:null,
      valu:''
    }
  },
  mounted() {
      this.observer= new IntersectionObserver(([entry]) => {
          if (entry && entry.isIntersecting) {
              this.$emit('intersect');
          }

      })

      this.observer.observe(this.$el);

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.observer {
    height: 10vh;

}

</style>
