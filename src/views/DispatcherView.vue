<template>
    <div id="orders">
      <div id="orderList">
        <div v-for="(order, key) in orders" v-bind:key="'order'+key">
          <div id="orderheader">
            #{{ key }}:
          <span v-for="(item, index) in order.orderItems" :key="'item' + index">
            <span v-if="item.amount > 0">
              {{ item.amount }} {{ item.name }},
            </span>
          </span>
          </div>
          <div id="orderinfo">
          Customer: {{ order.details.customerInfo.name }} |
          Email: {{ order.details.customerInfo.email }} |
          Gender: {{ order.details.customerInfo.gender }} |
          Payment method: {{ order.details.customerInfo.payment }}
          </div>
        </div>
        <button v-on:click="clearQueue">Clear Queue</button>
      </div>
      <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
          <div v-for="(order, key) in orders" v-bind:style="{ left: order.details.location.x + 'px', top: order.details.location.y + 'px'}" v-bind:key="'dots' + key">
            {{ key }}
          </div>
      </div>
    </div>
  </template>
  <script>
  import io from 'socket.io-client'
  const socket = io();
  
  export default {
    name: 'DispatcherView',
    data: function () {
      return {
        orders: null
      }
    },
    created: function () {
      socket.on('currentQueue', data =>
        this.orders = data.orders);
    },
    methods: {
      clearQueue: function () {
        socket.emit('clearQueue');
      }
    }
  }
  </script>
  <style>
  #orderList {
    top:1em;
    left:1em;
    position: absolute;
    z-index: 2;
    color:black;
    background: rgba(255,255,255, 0.5);
    padding: 1em;
  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    background-size: cover;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  
  #dots div {
    position: absolute;
    background: indianred;
    color: black;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
    font-family: "Times New Roman";
  }

  #orderheader {
    font-weight: bold;
  }
  #orderinfo {
    font-size: 100%;
    font-family: "Times New Roman";
  }
  </style>
  