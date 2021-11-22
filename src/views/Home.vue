<template>
  <header id="Header">
    <h1>Adams Premium Burgers Online</h1>
  </header>
  <body>
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
        <p>This is where you provide your information</p>
      </section>
      <section id="contact">
        <h4>Delivery information</h4>
        <p>
          <label for="Full name">Full name</label><br />
          <input
            type="name"
            id="name"
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
      </section>
      <section id="contact">
        <h4>Payment method</h4>
        <be>
          <select id="payment" v-model="payment">
            <option>Visa/Mastercard</option>
            <option>Klarna checkout</option>
            <option>Swish</option>
            <option>PayPal</option>
            <option>Send cash with mail-pidgeon</option>
          </select>
        </be>
      </section>
      <section id="contact">
        <h4>Gender</h4>
        <div>
          <input
            type="radio"
            id="Undisclosed"
            v-model="gender"
            value="Undisclosed"
          />
          <label for="Undisclosed">Undisclosed</label>
        </div>
        <div>
          <input type="radio" id="Female" v-model="gender" value="Female" />
          <label for="Female">Female</label>
        </div>
        <div>
          <input type="radio" id="Male" v-model="gender" value="Male" />
          <label for="Male">Male</label>
        </div>
        <div>
          <input
            type="radio"
            id="Non-binary"
            v-model="gender"
            value="Non-binary"
          />
          <label for="Non-binary">Non-binary</label>
        </div>
      </section>
    </section>
    <button v-on:click="addOrder" type="submit" class="submitbutton">
      Place order
    </button>
  </main>
  </body>
  <section id="mapWrapper">
    <div v-on:click="pinLocation" id="theMap">
      <div
        v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }"
        id="placedDot"
      >
        T
      </div>
    </div>
  </section>
  <hr>
    <footer>
        COPYRIGHT &copy;
        <br>
        If you are impressed by this webdesign and want to hire me contact me at "adamsundqvist22@gmail.com"
    </footer>
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
      payment: "Swish",
      gender: "Undisclosed",
      orderedBurgers: {},
      location: { x: 0, y: 0 },
    };
  },
  methods: {
    addToOrder: function (event) {
      console.log(event.name, event.amount);
      this.orderedBurgers[event.name] = event.amount;
    },
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },

    pinLocation: function (event) {
      let offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
    },

    addOrder: function () {
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        customerinfo: {
          name: this.fullName,
          email: this.email,
          payment: this.payment,
          gender: this.gender,
        },
        details: {
          x: this.location.x,
          y: this.location.y,
        },
        orderItems: this.orderedBurgers,
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
#mapWrapper {
  width: 1000px;
  height: 500px;
  position: relative;
  overflow: scroll;
}
#theMap {
  width: 1920px;
  height: 1078px;
  background: url(/img/polacks.jpg);
  cursor: crosshair;
}
#placedDot {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
}
</style>

