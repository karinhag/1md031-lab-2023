<template>

  <header>
    <div class="head">
      <img id="headerpic" src="https://www.amsterdamredlightdistricttour.com/wp-content/uploads/2022/10/Best-Burger-Amsterdam.jpg" alt="Burger header" >
      <h1 id="headline">Welcome to BurgerOnline</h1>
    </div>
  </header>
  <main>
    <section id ="burgerorder">
      <div>
        <h1>Burger Menu</h1>
        <div class="wrapper">
          <Burger v-for="burger in burgers"
                  v-bind:burger="burger"
                  v-bind:key="burger.name"
                  v-on:orderedBurgers="$event => addToOrder($event)"/>
        </div>
      </div>
      </section>



    <section id="customer">
      <h1> <font size= "+2">  <strong> Customer information </strong>
      </font> </h1>
      <p>
        <label for="name">First and last name</label><br>
        <input type="text" id=name v-model="customerInfo.name" required="required" placeholder="First and last name">
      </p>
      <p>
        <label for="email">E-mail Address</label><br>
        <input type="text" id="email" v-model="customerInfo.email" placeholder="E-mail Address">
      </p>

      <p>
        <label for="payment">Payment method</label><br>
        <select id="payment" v-model="customerInfo.payment">
          <option selected="selected">Debit card</option>
          <option>Credit card </option>
          <option>Swish</option>
          <option>Invoice</option>
        </select>
      </p>
      <p>
        <label for="gender">Gender</label><br>
        <input type="radio" id="female" v-model="customerInfo.gender" value="Female">
        <label for="gender">Female</label><br>
        <input type="radio" id="male" v-model="customerInfo.gender" value="Male">
        <label for="gender">Male</label><br>
        <input type="radio" id="not" v-model="customerInfo.gender" value="Do not wish to approve">
        <label for="gender">Do not wish to approve</label>
      </p>
      <div id="addesstext">Please click on your address: </div>
      <section class="mapWrapper">

        <div id="map" v-on:click="setLocation">
          <div id="dots">
            <div v-bind:style="{position: absolute, left: location.x + 'px', top: location.y + 'px'}" v-bind:key="'dots' + key">
              T
            </div>
          </div>
        </div>
      </section>
    </section>
    <button v-on:click="submit" type="submit">
      <img src="https://t3.ftcdn.net/jpg/06/46/48/96/360_F_646489630_ElUY2WiCDbxrt1eJeo387cUYEy2SJLKf.jpg" alt="order button" style="width:90px">;
    </button>
  </main>
  <hr>
  <footer>
    &copy; BurgerOnline 2023
  </footer>

</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();

function MenuItem(mn, pic, kCal, lac, glut, veg) {
  this.name = mn;
  this.kCal = kCal
  this.url = pic;
  this.lactose = lac;
  this.gluten = glut;
  this.veg = veg;
}
new MenuItem()


const burgerArray = [
  {name: "Cheese Burger", kCal: "600", url: "https://cdn-bk-se-ordering.azureedge.net/media/sryji43s/bkno0060_gourmet_triple_cheese_400x290_kiosk_produktbilde_singel_.png",lactose: true, gluten:true, veg:false},
  {name: "Halloumi Burger", kCal: "500", url: "https://cdn-bk-se-ordering.azureedge.net/media/ut4ej4et/400x290_halloumi-king-_singel.png",lactose: true, gluten: true, veg:true },
  {name: "Chicken Burger", kCal: "400", url: "https://cdn-bk-se-ordering.azureedge.net/media/p0efl3e0/bk_chilicheesecrispychicken_400x290_singel.png", lactose: false, gluten: false, veg:false},
];
 console.log(burgerArray)



export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      customerInfo: {
        name: "",
        email: "",
        payment: "Debit card",
        gender: "female"
    },
      orderedBurgers: [],
      location: {
        x: 0,
        y: 0
    },
      orderNumber: 0,

    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      this.location = {
        x: event.clientX - 10 -offset.x,
        y: event.clientY - 10 - offset.y
      };

    },
    submit: function() {
      console.log('Customer Information:', this.customerInfo);
      console.log('Ordered Burgers: ', JSON.parse(JSON.stringify(this.orderedBurgers)));
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: {
          location: this.location,
          customerInfo: this.customerInfo
        },
        orderItems: this.orderedBurgers
          },
      );
    },
    addToOrder: function (event) {
      const {name, amount} = event;
      const existingBurger = this.orderedBurgers.find(burger => burger.name === name);
      if (existingBurger) {
        existingBurger.amount = amount;
      }
      else {
        this.orderedBurgers.push({name, amount})
      }
      console.log('Ordered Burgers: ', this.orderedBurgers);
    },

    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top};
      this.location = {
        x: event.clientX - 10 -offset.x,
        y: event.clientY - 10 - offset.y
      };


    }

  }
}
</script>

<style>
  #map {
    background: url("/public/img/polacks.jpg");
    width: 1920px;
    height: 1078px;

  }
  .mapWrapper {
    overflow: scroll;
    height: 400px;
    background-color: white;
  }
  body {
    font-family: Futura;
    background-color: antiquewhite;
  }


  #customer {
    color: black;
    background-color: white;
    border: 10px groove;
    border-color: indianred;
    padding: 20px;

  }

  #burgerorder {
    padding: 20px;
    border: 10px groove;
    border-color: indianred;
    margin: 20px 100px;
    background-color: white;
  }

  button:hover {
    background-color: indianred;
    cursor: pointer;
  }

  section {
    margin: 20px 100px;
  }
  footer {
    margin: 0px 100px;
    font-size: 75%;
  }
  header {
    margin: 0px 100px;
  }
  button {
    margin: 0px 100px;
  }

  #burger {
    margin: 5px;
    padding: 10px;
  }
  header div{
    height: 200px;
    overflow: hidden;
    width: 100%;
    font-size: 150%;

  }
  #headerpic {
    opacity: 0.4;
    background-size: cover;
    background: url("https://www.amsterdamredlightdistricttour.com/wp-content/uploads/2022/10/Best-Burger-Amsterdam.jpg");

  }
  #headline {
    position: absolute;
    margin-top: -630px;
    margin-left: 30px;
    text-align: center;
    font-size: 280%;
    color: white;
    text-shadow: 0 0 20px aquamarine, 0 0 30px deeppink;
  }
  .wrapper {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 33.3% 33.3% 33.3%;
  }
  #addesstext {
    margin: 20px 100px;
    font-size: 120%;
  }

  html {
    width: 1200px}

  @media screen and (max-width: 800px) {
    h1 {
      font-size: 6vw;
    }
  }
</style>