<template>
  <div style>
    <h1>
      {{ burger.Name }}
    </h1>
    <img v-bind:src="burger.img" style="width:200px">
    <h4>Allergenes and caloric info</h4>
    <div>
      <ul class="allergi">
        <li>Contains lactose? {{ burger.Lactose }}</li>
        <li>Contains Gluten?{{ burger.Gluten }}</li>
        <li>{{ burger.KCal }}</li>
      </ul>
    </div>

    <button type="submit" v-on:click="addBurger">
      add burger
    </button>
    <button type="submit" v-on:click="removeBurger">
      remove burger
    </button>
    <div id="amount">{{amountOrdered}}</div>
  </div>
</template>

<script>
export default {
  name: 'Burger',
  props: {
    burger: Object
  },
  data: function () {
    return {
      amountOrdered: 0,
    }
  },
  methods:{
    removeBurger: function(){
      this.amountOrdered -=1
      this.$emit(
          'orderedBurger',
          {
            name:   this.burger.Name,
            amount: this.amountOrdered
          }
      );
    },
    addBurger: function () {
      this.amountOrdered += 1;
      this.$emit(
          'orderedBurger',
          {
            name:   this.burger.Name,
            amount: this.amountOrdered
          }
      );
    }
}}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#amount{
  margin-left: 50px;
}
</style>
