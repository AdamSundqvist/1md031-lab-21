<template>
  <div id="burgerID">
    <h4>{{ burger.name }}</h4>
    <img id="imgID" v-bind:src="burger.image" />
    <ul>
      <p>Calories: {{ burger.kCal }} kCal</p>
      <span v-if="burger.lactose == true">
        <li><span class="allergies"> Contains lactose </span></li>
      </span>
      <span v-if="burger.gluten == true">
        <li><span class="allergies"> Contains gluten </span></li>
      </span>
      <span v-if="burger.gluten == false && burger.lactose == false">
        <li>Does not contain allergenics</li>
      </span>
    </ul>
    <p>
      Amount :
      <button v-on:click="amountOrdered -= 1" type="button" id="amountButtons">
        -
      </button>
      {{ amountOrdered }}
      <button v-on:click="amountOrdered += 1" type="button" id="amountButtons">
        +
      </button>
    </p>
  </div>
</template>

<script>
export default {
  name: "Burger",
  props: {
    burger: Object,
  },
  data: function () {
    return {
      amountOrdered: 0,
    };
  },
  methods: {
    addBurger: function () {
      this.amountOrdered += 1;
      this.$emit("orderedBurger", {
        name: this.burger.name,
        amount: this.amountOrdered,
      });
    },
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
#amountButtons {
  border: 2px solid lightgrey;
  background-color: #fff;
  font-size: 15px;
  font: bold;
  height: 2em;
  width: 2em;
  border-radius: 999px;
  position: relative;
  margin: 5px;
}
</style>
