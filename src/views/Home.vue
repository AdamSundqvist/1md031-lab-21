<template>
  <div>
    Burgers
    <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"/>
  </div>
  <div id="map" v-on:click="addOrder">
    click here
  </div>
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'

const socket = io();

//object contructor menuItem goes here
function menuItem(name, imgURL, kCal,gluten,lactose) {
  this.name = name;
  this.image = imgURL;
  this.calories = kCal;
  this.gluten = gluten;
  this.lactose = lactose;
}

const burgerarray [];
burgerarray[0] = new menuItem("Original Tasty","https://www.foodrepublic.com/wp-content/uploads/2012/03/033_FR11785-700x466.jpg"
,500,true,true);
burgerarray[1] = new menuItem("El Fuego","https://www.nrn.com/sites/nrn.com/files/styles/article_featured_retina/public/uploads/2014/10/et-burgerdiablopromo_0.jpg?itok=hYC7WpGi"
,600,true,true);
burgerarray[2] = new menuItem("The GreenPeace","https://www.thespruceeats.com/thmb/KAgMssHoQUmx30uuYL_FTahXA0A=/2048x1360/filters:fill(auto,1)/vegan-mushroom-bean-burger-recipe-3378623-13_preview1-5b241897fa6bcc0036d2c9bf.jpeg"
,400,false,false);


export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: [ {name: "small burger", kCal: 250},
                 {name: "standard burger", kCal: 450},
                 {name: "large burger", kCal: 850}
               ]
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}
</script>

<style>
  #map {
    width: 300px;
    height: 300px;
    background-color: red;
  }
</style>
