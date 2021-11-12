<template>
  <div>
    <div id="burgerOne">
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
    {{ burger.name }} {{ burger.kCal }} {{ burger.imgURL }}
    {{ burger.lactose }} {{ burger.gluten }}
  </div>


  
</template>

<script>
export default {
  name: "Burger",
  props: {
    burger: Object,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#burgerOne {
  width: auto;
  height: 200px;
}
</style>
