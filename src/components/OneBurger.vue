<template>
  <div class="wrapper">
    <div class ="box a">
      <div class="burgerName">{{ burger.name }} </div>
    <img v-bind:src="burger.url" alt="burger.name" style="width:200px;">
      <ul>
        <li id="caltext">Containts {{burger.kCal}} kCal </li>
      <li class="allergy" v-if="burger.gluten">Gluten</li>
      <li class="allergy" v-if="burger.lactose">Lactose</li>
      <li class="veg" v-if="burger.veg">Vegetarian</li>
    </ul>
      <div id="ordertext">Amount ordered: {{amountOrdered}}</div>
      <button v-on:click="increase" type="button">+
      </button>
      <button v-on:click="decrease" type="button">-
      </button>
  </div>
  </div>
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: {
        type: Object,
        required: true,
      }
    },

    data: function() {
      return {
        amountOrdered: 0,
      }
    },
    methods: {
      increase() {
        this.amountOrdered += 1;
        this.$emit('orderedBurgers',{name: this.burger.name, amount: this.amountOrdered});
        console.log('Burger added', this.burger.name);
      },
      decrease() {
        if (this.amountOrdered > 0) {
          this.amountOrdered -= 1;
          this.$emit('orderedBurger',{name: this.burger.name, amount: this.amountOrdered});
          console.log('Burger removed:', this.burger.name);
        }
        else {
          this.amountOrdered = 0;
        }

      }
    }


  }

  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  .allergy {
    color: darkred;
    font-size: smaller;
    font-weight: bold;
    font-family: Verdana ;
  }
  .veg {
    color: darkgreen;
    font-size: smaller;
    font-weight: bold;
    font-family: Verdana;
  }


  .wrapper {
      display: grid;
      grid-gap: 10px;
      grid-template-columns: 33% 33% 33%;
    background-color: antiquewhite;
    border-radius: 5px;
    padding: 20px;
    white-space: nowrap;
    }

  .burgerName {
    margin: 20px;
    font-size: 150%;
    white-space: nowrap;
    font-weight: bold;
  }
  #caltext {
    font-family: Verdana;
    font-weight: bold;
    text-align: center;
    font-size: smaller
  ;
  }
  #ordertext {
    font-family: Verdana;
    font-size: smaller;
    font-weight: bold;
    color: indianred;

  }
  button {
    margin: 5px;
  }



  </style>

