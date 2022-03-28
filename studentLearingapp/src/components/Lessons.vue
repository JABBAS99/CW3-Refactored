<template>
  <div class="container">
    <div class="row">
      <div class="d-flex flex-wrap">
        <div
          v-for="(lesson , index) in Lessons"
          :key="index"
          class="card lessoncard text-center mt-5"
        >
          <figure>
            <img
              :src="require(`${lesson.Image}`)"
              class="img-thumbnail card-img-top card-image"
              height="200"
              width="200"
            />
          </figure>
          <i :class="`fa ${lesson.Icon}`"></i>
          <h3>Subject: {{lesson.Subject}}</h3>
          <h5>Location: {{lesson.Location}}</h5>
          <p>Price: £{{lesson.Price}}</p>
          <p>Spaces: {{lesson.Spaces}}</p>
          <button class="btn btn-primary" @click="handleAddItemToCart(lesson)">
            Add To
            Cart
          </button>
          <!-- :disabled="!canAddToCart(lesson)" -->
          <span v-if="lesson.Spaces == 0">All out!</span>
          <br />
          <br />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Lessons",
  props: {
    Lessons: Array,
    handleAddItemToCart: Function,
  },
  methods: {
    getImgUrl(pic) {
      return require("../assets/images/" + pic);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
}
a {
  color: #42b983;
}
.lessoncard {
  width: 30%;
  margin-right: 30px;
  padding: 10px;
}
.card-image {
  width: 100%;
  height: 200px !important;
}
</style>
