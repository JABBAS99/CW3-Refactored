<template>
  <div>
    <div class="text-center">
      <button class="btn btn-secondary mt-5" @click="showCheckout" v-show="showCart">
        {{cartItemCount}}
        <span>
          <i class="fas fa-shopping-basket"></i>
        </span> Checkout
      </button>
    </div>

    <div v-if="showProduct">
      <div class="text-center mt-5">
        <div class="row">
          <div class="col-sm-12 col-md-5 offset-md-1">
            <input
              class="form-control me-2"
              v-model="search"
              type="search"
              placeholder="Search Lesson.."
            />
          </div>
          <div class="col-sm-12 col-md-6">
            <button class="btn btn-success" @click="sortPriceByLow">
              <span>
                <i class="fas fa-chevron-down"></i>
              </span> SortByPriceLow
            </button>
            <button class="btn btn-success ml-1" @click="sortPriceByHigh">
              <span>
                <i class="fas fa-chevron-up"></i>
              </span>
              SortByPriceHigh
            </button>
          </div>
        </div>
      </div>
      <lessons :Lessons="filteredLessons" :handleAddItemToCart="addcart" />
    </div>
    <div v-else class="text-center mt-5">
      <check-out
        :backtoLessons="backtoLessons"
        :showCheckout="showCheckout"
        :cartItemCount="cartItemCount"
        :cart="cart"
        :removeFromCart="removeFromCart"
        :placeOrder="placeOrder"
      />
    </div>
  </div>
</template>

<script>
import CheckOut from "./CheckOut.vue";
import Lessons from "./Lessons.vue";
// import axios from "axios";
export default {
  name: "root",
  data() {
    return {
      Lessons: [],
      search: "",
      showProduct: true,
      cart: [],
    };
  },
  components: {
    Lessons,
    CheckOut,
  },
  mounted() {
    fetch("https://student-app-mbou.herokuapp.com/lessons").then(res=>res.json()).then((res) => {
      // console.log(res,'asdasdasd')
      this.Lessons = res;
    });
  },
  computed: {
    cartItemCount() {
      return this.cart.length || "";
    },
    showCart() {
      return this.cart.length > 0;
    },
    backtoLessons() {
      return this.cart.length == 0;
    },

    filteredLessons: function () {
      return this.Lessons.filter((lesson) => {
        return (
          lesson.Subject.toLowerCase().match(this.search.toLowerCase()) ||
          lesson.Location.toLowerCase().match(this.search.toLowerCase()) ||
          lesson.Price.toString().match(this.search.toString())
        );
      });
    },
  },
  methods: {
    addcart(lesson) {
      this.cart.push(lesson);
      lesson.Spaces -= 1;
    },
    showCheckout() {
      this.showProduct = !this.showProduct;
    },
    canAddToCart(lesson) {
      return lesson.Spaces > 0;
    },
    getImgUrl(pic) {
      return require("../assets/images/" + pic);
    },
    cartCount(id) {
      let count = 0;
      for (let i = 0; i < this.cart.length; i++) {
        if (this.cart[i] === id) {
          count++;
        }
      }
      return count;
    },
    sortPriceByLow() {
      function compare(a, b) {
        if (a.Price > b.Price) return 1;
        if (a.Price < b.Price) return -1;
        return 0;
      }
      return this.Lessons.sort(compare);
    },
    sortPriceByHigh() {
      function compare(a, b) {
        if (a.Price > b.Price) return -1;
        if (a.Price < b.Price) return 1;
        return 0;
      }
      return this.Lessons.sort(compare);
    },
    placeOrder() {
      alert("Order submitted!!!");
    },
    removeFromCart(lesson) {
      console.log("get called..");
      console.log(lesson);
      this.cart = this.cart.filter((i) => i.id !== lesson.id);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>

/////////////