<template>
  <div>
    <div v-show="backtoLessons">
      <button class="btn btn-secondary" v-on:click="showCheckout">
        {{cartItemCount}}
        <span>
          <i class="fas fa-arrow-left"></i>
        </span> Back To Lessons
      </button>
    </div>
    <h1>Checkout</h1>

    <h1>Lessons in basket</h1>

    <div class="container">
      <div class="row">
        <div class="card lessoncard" v-for="(lesson , index) in cart" :key="index">
          <img
            :src="require(`${lesson.Image}`)"
            class="img-thumbnail card-img-top card-image"
            height="200"
            width="200"
          />
          <h3>{{lesson.Subject }}</h3>
          <h4>{{ lesson.Location }}</h4>
          <h6>£{{ lesson.Price }}</h6>
          <button class="btn btn-danger" @click="removeFromCart(lesson)">
            <i class="fa fa-trash"></i>
            Delete
          </button>
          <br />
          <br />
        </div>
      </div>

      <h1 class="mt-5">Checkout</h1>
      <div class="row">
        <div class="col-8 offset-2">
          <p>
            <strong>First Name:</strong>
            <input class="form-control" v-model="order.firstName" />
          </p>
          <p>
            <strong>Phone Number:</strong>
            <input class="form-control" v-model="order.phoneNumber" />
          </p>
          <div v-show="!validateName || !validateNumber">
            <p>Error: Please make sure name is only letters and Phone is only numbers</p>
          </div>
          <h3>Order information</h3>
          <p>First name: {{order.firstName}}</p>
          <p>Phone Number: {{order.phoneNumber}}</p>
          <div v-show="validateNumber && validateName">
            <button class="btn btn-success" @click="placeOrder">Place Order</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "checkout",
  props: [
    "cart",
    "backtoLessons",
    "showCheckout",
    "cartItemCount",
    "removeFromCart",
    "placeOrder",
  ],
  computed: {
    validateName() {
      return /^[a-zA-Z]+$/.test(this.order.firstName);
    },
    validateNumber() {
      return /^[0-9]+$/.test(this.order.phoneNumber);
    },
  },
  data() {
    return {
      order: {
        firstName: "",
        phoneNumber: "",
      },
    };
  },
};
</script>
<style scoped>
.lessoncard {
  width: 30%;
  margin-right: 20px;
  padding: 10px 20px;
}
.card-image {
  width: 100%;
  height: 200px !important;
}
</style>