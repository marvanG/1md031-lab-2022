<template>
    <div id="orders">
      <div id="orderList">
        <div>
         
          
             <div v-for="(order, key) in orders">
    #{{ key }}: 
    
    <div v-for="(amount,key) in order.orderItems" >
    <div v-if="amount != '0' ">{{key}}: {{amount}}</div>
    </div>
    <p id="customer">{{(order.customer.name)}} ( {{(order.customer.email)}}, {{(order.customer.payMethod)}}, {{(order.customer.gender)}})  </p>
  </div>

          
        
          
         
        
        </div>
        <button v-on:click="clearQueue">Clear Queue</button>
      </div>
      <div id="dots" v-bind:style="{ background: 'url(' + require('../../public/img/polacks.jpg')+ ')' }">
          <div v-for="(order, key) in orders" v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px'}" v-bind:key="'dots' + key">
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
        orders: null,
        
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
    width:1920px;
    height: 1078px;
    cursor: crosshair;
  }
  
  #dots div {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }
  #customer{
    font-style: italic;
  }
  </style>
  