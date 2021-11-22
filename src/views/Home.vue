<template>
  <main>
    <section id="burgersection">
      <h3>Burger selection</h3>
      <p>This will be the burger selection area</p>
      <div class="wrapper">
        <Burger
          v-for="burger in burgers"
          v-bind:burger="burger"
          v-bind:key="burger.name"
          v-on:orderedBurger="addToOrder($event)"
        />
      </div>
    </section>
    <section style="clear: left" id="customerinfosection">
      <section id="contact">
        <h3>Customer Information</h3>
        <p>Content</p>
      </section>
      <section id="contact">
        <h4>Delivery information</h4>
        <p>
          <label for="Full name">Full name</label><br />
          <input
            type="text"
            id="Full name"
            v-model="fullName"
            required="required"
            placeholder="First and Last name"
          />
          {{ fullName }}
        </p>
        <p>
          <label for="E-mail">Email</label><br />
          <input
            type="email"
            id="Email"
            v-model="email"
            placeholder="Enter your email"
          />
          {{ email }}
        </p>
        <p>
          <label for="Street name">Street</label><br />
          <input
            type="adress"
            id="adress"
            v-model="streetName"
            required="required"
            placeholder="Street name"
          />
          {{ streetName }}
        </p>
        <p>
          <label for="House number">House</label><br />
          <input
            type="number"
            id="housenumber"
            v-model="houseNumber"
            required="required"
            placeholder="House number"
          />
        </p>
      </section>
      <section id="contact">
        <h4>Payment method</h4>
        <be>
          <select id="Payment" name="Payment">
            <option>Visa/Mastercard</option>
            <option>Klarna checkout</option>
            <option>Swish</option>
            <option>PayPal</option>
            <option selected="selected">Send cash with mail-pidgeon</option>
          </select>
        </be>
      </section>
      <section id="contact">
        <h4>Gender</h4>
        <div>
          <input
            type="radio"
            id="undisclosured"
            name="gender"
            value="undisclosured"
            checked
          />
          <label for="Undisclosured">Undisclosured</label>
        </div>
        <div>
          <input type="radio" id="Female" name="gender" value="Female" />
          <label for="Female">Female</label>
        </div>
        <div>
          <input type="radio" id="Male" name="gender" value="Male" />
          <label for="Male">Male</label>
        </div>
        <div>
          <input
            type="radio"
            id="Non-binary"
            name="gender"
            value="Non-binary"
          />
          <label for="Non-binary">Non-binary</label>
        </div>
      </section>
    </section>
    <button v-on:click="submitted" type="submit" class="submitbutton">
      Place order
    </button>
  </main>
  <section id="mapwrapper">
    <div
      v-on:click="addOrder"
      v-bind:style="{
        left: location.x + 'px',
        top: location.y + 'px',
      }"
    >
      T
    </div>
  </section>
</template>

<script>
import Burger from "../components/Burger.vue";
import io from "socket.io-client";
import menu from "../assets/menu.json";
const socket = io();
const burgerArray = menu;
console.log(burgerArray);
export default {
  name: "Home",
  components: {
    Burger,
  },
  data: function () {
    return {
      burgers: burgerArray,
      fullName: "",
      email: "",
      streetName: "",
      houseNumber: "",
      orderedBurger: {},
      location: { x: 0, y: 0 },
    };
  },
  methods: {
    submitted: function () {
      console.log(
        this.fullName,
        this.email,
        this.streetName,
        this.houseNumber,
        this.orderedBurger
      ); //eventuellt hs this["orderedBurger"]
    },
    addToOrder: function (event) {
      this.orderedBurger[event.name] = event.amount;
    },
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    addOrder: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top, //offset är ett objekt och x och y är keyvaluepairs
      };
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: event.clientX - 10 - offset.x,
          y: event.clientY - 10 - offset.y,
        },
        orderItems: ["Beans", "Curry"], //hela details är ett objekt som skickas
      });
    },
  },
};
</script>

<style>
#Header {
  padding: 100px;
  background-image: url(https://debaser.se/wp-content/themes/debaser/img/food/barbrooklyn2019/02.jpg);
  background-position: 50%;
  text-align: center;
  font-size: 2em;
  font-family: fantasy;
  -webkit-text-fill-color: darkslateblue; /* Will override color (regardless of order) */
  -webkit-text-stroke-width: 1px;
  -webkit-text-stroke-color: white;
  margin-bottom: 5px;
}
#burgersection {
  color: white;
  background-color: black;
  overflow: auto;
  border: dashed;
  margin-bottom: 5px;
  padding: 5px;
}
.wrapper {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: repeat(3, 1fr);
}
#customerinfosection {
  border: dashed;
}
.submitbutton {
  font-size: 20px;
  padding: 10px 15px;
  text-align: center;
  cursor: pointer;
  outline: none;
  color: #fff;
  background-color: #04aa6d;
  border: none;
  border-radius: 15px;
  box-shadow: 0 9px #999;
  margin-bottom: 10px;
  margin-top: 10px;
}
.submitbutton:hover {
  background-color: #3e8e41;
}
.submitbutton:active {
  background-color: #3e8e41;
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}
#contact {
  margin-left: 10px;
  margin-bottom: 10px;
}
#mapwrapper {
  position: relative;
  margin: 0;
  padding: 0;
  background: url(/img/polacks.jpg);
  background-repeat: no-repeat;
  width:1920px;
  height: 1078px;
  cursor: crosshair;
  overflow: scroll;
}
#mapwrapper div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
}
</style>

