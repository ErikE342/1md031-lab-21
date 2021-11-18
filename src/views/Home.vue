<template>
  <img src="https://www.londonkoll.se/wp-content/uploads/2014/11/Pubar-London-1200x630px.jpg" class="headline foto">
  <header class="headline" style="margin-top:-300px;">
    <h1>Burgers Online</h1>
  </header>
  <main>
    <section id="burger-select">
      <h1 class="headed">
        Select Burger
      </h1>
      <div class="headed">
        all the selections
      </div>
      <div class="wrapper">

          <Burger v-for="burger in burgers"
                  v-bind:burger="burger"
                  v-bind:key="burger.name"
                  v-on:orderedBurgers="addToOrder($event)"/>

      </div>
    </section>

    <section style="clear:left;" id="personal-info">
      <h2>
        Customer Information
      </h2>
      <p>
        Call us at xxx-xxx xx xx if there is a problem
      </p>
      <h2>
        Personal Information
      </h2>
      <p>
        <label for="firstname">First and Last</label><br>
        <input type="text" id="firstname" v-model="fn" required="required" placeholder="First name and Last name">
        {{fn}}
      </p>
      <p>
        <label for="Street">Name</label><br>
        <input type="text" id="Street" v-model="ln" placeholder="Street Name">
        {{ln}}
      </p>
      <p>
        <label for="Street">Mail</label><br>
        <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
        {{em}}
      </p>
      <p>
        <label for="Adress">Number</label><br>
        <input type="number" id="Adress" v-model="Nm" required="required" placeholder="address Number">
        {{Nm}}
      </p>
      <p>
        <label >Payment Method</label><b>
        <select id="recipient" v-model="rcp">
          <option>PayPal</option>
          <option>Faktura</option>
          <option>Swish</option>
          <option>Credit card</option>
        </select>
        {{rcp}}
      </b>
      </p>
      <p>
        Gender
      </p>
      <p>
        Un-Disclosed
        <input type="radio" id="Un-Disclosed" v-model="Rd" checked="checked" name="GenderButton" value="Un-disclosed">
      </p>
      <p>
        Male
        <input type="radio" id="Male" v-model="Rd" name="GenderButton" value="male">
      </p>
      <p>
        Female
        <input type="radio" id="Female" v-model="Rd" name="GenderButton" value="Female">

      </p>
      <p>
        Non-Binary
        <input type="radio" id="Non-Binary" v-model="Rd"  name="GenderButton" value="Non-Binary">
      </p>
      {{Rd}}

    </section>
    <button type="submit" v-on:click="addToOrder">
      <img src="https://www.creativefabrica.com/wp-content/uploads/2020/02/10/Delivery-Logo-Graphics-1-5-580x386.jpg" alt="span" title="burger" style="width: 50px;">
      AddToOrder (for now)
    </button>
  </main>

  <hr>
  <footer>
    &copy; Burger site
  </footer>
  <div id="map" v-on:click="addOrder">
    click here
  </div>
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from "../assets/menu.json"

const socket = io();

/*function MenuItem(Name, KCal, url, Gluten, Lactose){
  this.name = Name;
  this.kCal = KCal;
  this.GlutenAl = Gluten;
  this.LactoseAl = Lactose;
  this.img = url;
}*/

let Burgers = menu;
    /*[new MenuItem("Fire Burger","600 calories", "https://cdn-bk-se-ordering.azureedge.net/media/x0ljohbg/bk-kiosk-checkout-machine-400x290_singel_roasted_onion_singlebeef.png" ,"gluten:yes", "Lactose: yes"),
  new MenuItem("Blue Burger", "500 calories", "https://www.max.se/globalassets/images/burgers/burgers-frisco-burger.png?width=1160&height=652","gluten:yes", "Lactose: yes"),
  new MenuItem("Green Burger",  "400 calories", "https://www.santamariaworld.com/optimized/maximum/globalassets/_recipes/bbq/kentucky_burger.jpg","gluten:no", "Lactose: no")];
*/
console.log(Burgers);


export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: Burgers,
      fn: "",
      ln: "",
      em: "",
      Nm: "",
      rcp: "",
      Rd: "",
      OrderedBurgers: {}


              /*[ {name: "small burger", kCal: 250},
                 {name: "standard burger", kCal: 450},
                 {name: "large burger", kCal: 850}
               ]*/
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);
    },
    addOrder: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
      socket.emit("addOrder", {
            orderId: this.getOrderNumber(),
            details: {
              x: event.clientX - 10 - offset.x,
              y: event.clientY - 10 - offset.y
            },
            orderItems: ["Beans", "Curry"]
          }
      );
    },
    addToOrder: function (event) {
      console.log(this.fn, this.ln, this.em, this.Nm, this.rcp, this.Rd, this.OrderedBurgers)
      this.OrderedBurgers[event.name] = event.amount;
    }
  }
}
</script>

<style>
/*@import "reset.css";*/
@import url("https://fonts.googleapis.com/css?family=Droid+Serif|Share+Tech+Mono");

body {
  font-family: Arial;
  font-size: 12pt;
}

#map {
  width: 300px;
  height: 300px;
  background-color: red;
}
.allergi{
  color: #ff5500;
  text-transform: uppercase;
}
#burger-select{
  background-color: black;
  color: white;
  border-style: dashed;
  border-color: white;
}
#personal-info{
  border-color: black;
  border-style: dashed;
}
button:hover {
  background-color: blue;
  cursor: pointer;
}
button {
  margin:0px 40px 0px 40px;
}
.headline{
  margin:0px 40px 0px 40px;
  overflow:hidden;
}
.foto{
  opacity: 0.4;
  height: 400px;
  width: 95%;
}
headline, header{
  position:absolute;
}

section {
  margin:0px 40px 0px 40px;
}

.headed{
  margin-left:40px;
}
.wrapper {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: 200px 200px 200px;
}


</style>
