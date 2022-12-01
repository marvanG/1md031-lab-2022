<template>
   
 


    <div>
      <div class="burgerstyle">
      <h3>{{burger.name}}</h3>
      <div id="orderAmount">
        <button id="minusBurger" @click="minusBurger()">-</button>
        <button id="plusBurger" @click="plusBurger()">+</button>
        <p id="amount">Order quantity: {{amountOrdered}}</p>
      </div>
      
      
      <img v-bind:src="burger.img" title="burger" alt="burger" style="width: 200px;" />
      </div>
    
    
      <ul>
        <li>{{burger.description1}}</li>
        <li>{{burger.description2}}</li>
        <li>{{burger.kcal}} calories</li>
       </ul>

    </div>
  </template>
  
  <script>
  

  export default {
    name: 'Burger',
    
    props: {
      burger: Object,

    },
    data: function () {
  return {
    amountOrdered: 0,
  }
},
methods: {
  minusBurger () {
    this.amountOrdered -= 1;
    this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
  );

  },
  plusBurger () {
    this.amountOrdered += 1;
    this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
  );

  }

}
  
  }


  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

  #amount {
    margin-left:10px;
    

  }

  #orderAmount {
    display: flex;
    align-items: center;
    
  }
  #minusBurger:hover{
    
    background-color: red;

  }
  #plusBurger:hover{
    background-color: green;
  }
  
  </style>
  