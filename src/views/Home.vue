<template>
  <img src="https://www.londonkoll.se/wp-content/uploads/2014/11/Pubar-London-1200x630px.jpg" class="headline foto" alt="pub">
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
                  v-on:orderedBurger="addToOrder($event)"/>

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
      </p>
      <p>
        <label for="email">Mail</label><br>
        <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
      </p>

      <p>
        <label >Payment Method</label><b>
        <select id="recipient" v-model="rcp">
          <option>PayPal</option>
          <option>Faktura</option>
          <option>Swish</option>
          <option>Credit card</option>
        </select>
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

    </section>
    <button type="submit" v-on:click="addOrder">
      <img src="https://www.creativefabrica.com/wp-content/uploads/2020/02/10/Delivery-Logo-Graphics-1-5-580x386.jpg" alt="span" title="burger" style="width: 50px;">
      Place Order
    </button>
  </main>
  <hr>
  <footer>
    &copy; Burger site
  </footer>
  <div id="mapscroll">
    <div id="map" v-on:click="setLocation">
      click here
      <div id="dotsen" v-bind:style="{ left: location.x + 'px', top: location.y + 'px'}">
        T
      </div>
    </div>
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


export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: Burgers,
      fn: "",
      em: "",
      rcp: "",
      Rd: "",
      orderedBurgers: {},
      location: {
        x: 0,
        y: 0
      }

              /*[ {name: "small burger", kCal: 250},
                 {name: "standard burger", kCal: 450},
                 {name: "large burger", kCal: 850}
               ]*/
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 1000);
    },
    addOrder: function () {
      /*var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };*/
      socket.emit("addOrder", {
            orderId: this.getOrderNumber(),
            details: {
              x: this.location.x,
              y: this.location.y,
            },
            orderItems: [this.orderedBurgers],
            PersonalInfo: [this.fn, this.em, this.rcp, this.Rd]
          },
      );
      console.log("Order sent to dispatcher")
    },
    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
      this.location = {
        x: event.clientX - 10 - offset.x,
        y: event.clientY - 10 - offset.y
      }
      console.log("set new location")
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
      console.log(this.orderedBurgers);
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
  position: relative;
  width: 1920px;
  height: 1078px;
  background: url("/img/polacks.jpg");
}
#mapscroll{
  overflow:scroll;
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
#dotsen {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}


</style>
