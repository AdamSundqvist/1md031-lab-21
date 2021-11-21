<template>
 <div id="burgerID">
        <h4>{{burger.name}}</h4>
        <img id="imgID" v-bind:src="burger.image"/>
   <ul>
     <p>Calories: {{burger.kCal}} kCal</p>
     <span v-if="burger.lactose == true"> <li> <span class="allergies"> Contains lactose </span> </li> </span>
     <span v-if="burger.gluten == true"> <li> <span class="allergies"> Contains gluten </span> </li> </span>
     <span v-if="burger.gluten == false && burger.lactose == false"> <li> Does not contain allergenics </li> </span>
   </ul>
   <p>Amount ordered: {{amountOrdered}}</p>
   <button type="button" id="amountButtons">-</button>
   <button type="button" id="amountButtons">+</button>
  </div>

</template>

<script>
export default {
  name: "Burger",
  props: {
    burger: Object,
  },
  data: function() {
    return {
      amountOrdered: 0,
    }
  },
};  
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#imgID {
  height: 200px;
  width: auto;
  }
#burgerID {
  margin-left: 40px;
}
.allergies {
  font-weight: bold;
}

</style>
