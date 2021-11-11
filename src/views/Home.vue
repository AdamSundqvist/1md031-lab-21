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

//object contructor menuItem goes here (task 1)
function menuItem(name, imgURL, kCal,gluten,lactose) {
  this.name = name;
  this.image = imgURL;
  this.calories = kCal;
  this.gluten = gluten;
  this.lactose = lactose;
}

let burgerOne = new menuItem("Original Tasty","url",500,true,true)
let burgerTwo = new menuItem("El Fuego","url",600,true,true)
let burgerThree = new menuItem("The GreenPeace","url",400,false,false)

let burgerArray = [burgerOne,burgerTwo,burgerThree];

//("Original Tasty","url",500,true,true)
//("El Fuego","url",600,true,true);
//("The GreenPeace","url",400,false,false);

console.log(burgerArray);

//end of task 1

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
