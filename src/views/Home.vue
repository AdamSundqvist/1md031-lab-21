<template>
  <header id="Header">
    <h1>Adams Premium Burgers Online</h1>
  </header>
  <main>
    <section id="burgersection">
      <h3>Burger selection</h3>
      <p>This will be the burger selection area</p>
      <div class="wrapper">
        <div class="burgerOne">
          <h4>Original Tasty</h4>
          <img
            src="https://www.foodrepublic.com/wp-content/uploads/2012/03/033_FR11785-700x466.jpg"
            width="auto"
            height="200px;"
            alt="Orginal Burger"
          />
          <h4>Ingredients</h4>
          <ul>
            <li>Bread (glutenfree option is available)</li>
            <li>Patty</li>
            <li>Cheese (lactose free option is available</li>
            <li>Lettuce</li>
            <li>Tomato</li>
            <li>Red onion</li>
            <li>Pickles</li>
          </ul>
        </div>
        <div class="burgerTwo">
          <h4>El Fuego</h4>
          <img
            src="https://www.nrn.com/sites/nrn.com/files/styles/article_featured_retina/public/uploads/2014/10/et-burgerdiablopromo_0.jpg?itok=hYC7WpGi"
            width="auto"
            height="200px"
            alt="Spicy burger"
          />
          <h4>Ingredients</h4>
          <ul>
            <li>Bread (glutenfree option is available)</li>
            <li>Patty</li>
            <li>Cheese (lactose free option is available)</li>
            <li>Lettuce</li>
            <li>Tomato</li>
            <li>Spicy jalapeños</li>
            <li>Caramellized onions</li>
          </ul>
        </div>
        <div class="burgerThree">
          <h4>The GreenPeace</h4>
          <img
            src="https://www.thespruceeats.com/thmb/KAgMssHoQUmx30uuYL_FTahXA0A=/2048x1360/filters:fill(auto,1)/vegan-mushroom-bean-burger-recipe-3378623-13_preview1-5b241897fa6bcc0036d2c9bf.jpeg"
            width="auto"
            height="200px"
            alt="Vegan burger"
          />
          <h4>Ingredients</h4>
          <ul>
            <li>Bread (glutenfree option is available)</li>
            <li>Mushroom patty</li>
            <li>Vegan cheese</li>
            <li>Lettuce</li>
            <li>Cucumber</li>
          </ul>
        </div>
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
            name="fn"
            required="required"
            placeholder="First and Last name"
          />
        </p>
        <p>
          <label for="E-mail">Email</label><br />
          <input
            type="email"
            id="Email"
            name="em"
            placeholder="Enter your email"
          />
        </p>
        <p>
          <label for="Street name">Street</label><br />
          <input
            type="adress"
            id="email"
            name="adress"
            required="required"
            placeholder="Street name"
          />
        </p>
        <p>
          <label for="House number">House</label><br />
          <input
            type="number"
            id="number"
            name="number"
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
    <button type="submit" class="submitbutton">Place order</button>
  </main>
  <hr />
  <footer>
    COPYRIGHT &copy;
    <br />
    If you are impressed by this webdesign and want to hire me contact me at
    "adamsundqvist22@gmail.com"
  </footer>
  <div>
    Burgers
    <Burger v-for="burger in burgerArray"
            v-bind:burger="burger" 
            v-bind:key="burger.name"/>
  </div>
  <div id="map" v-on:click="addOrder">
    click here
  </div>
</template>

<script>
import Burger from "../components/Burger.vue";
import io from "socket.io-client";

const socket = io();

//object contructor menuItem goes here (task 1)
function menuItem(name, imgURL, kCal, gluten, lactose) {
  this.name = name;
  this.image = imgURL;
  this.kcal = kCal;
  this.gluten = gluten;
  this.lactose = lactose;
}

let burgerOne = new menuItem("Original Tasty", "url", 500, true, true);
let burgerTwo = new menuItem("El Fuego", "url", 600, true, true);
let burgerThree = new menuItem("The GreenPeace", "url", 400, false, false);

let burgerArray = [burgerOne, burgerTwo, burgerThree];

//end of task 1

export default {
  name: "Home",
  components: {
    Burger,
  },
  data: function () {
    return {
      burgers: burgerArray,
    };
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    addOrder: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          x: event.clientX - 10 - offset.x,
          y: event.clientY - 10 - offset.y,
        },
        orderItems: ["Beans", "Curry"],
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
     color: white;
     background-color: black;
}

.burgerOne {
    grid-column: 1 ;
}

.burgerTwo {
    grid-column: 2 ;
}

.burgerThree {
    grid-column: 3 ;
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
    background-color: #04AA6D;
    border: none;
    border-radius: 15px;
    box-shadow: 0 9px #999;
    margin-bottom: 10px;
    margin-top: 10px;
}
.submitbutton:hover {background-color: #3e8e41}

.submitbutton:active {
  background-color: #3e8e41;
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}

#contact {
    margin-left: 10px;
    margin-bottom: 10px;
}

#map {
  width: 300px;
  height: 300px;
  background-color: red;
}
</style>
