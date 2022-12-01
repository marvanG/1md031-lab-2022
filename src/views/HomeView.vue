<template>
     <body>
    <header>
    <a id="banner"><img src="https://idsb.tmgrup.com.tr/ly/uploads/images/2020/04/29/33077.jpg" alt="header"></a>
    <h1 id="headline"> Welcome to BurgerApocalypse</h1>
    </header>

    <main>
    <section class="selection">
        <div class="burgerstyle">
                <h2>The four burgers of the apocalypse </h2>
                <p>This is our burger selection. Please select burgers to order.</p>

            </div>
      <div class="wrapper">
        <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-on:orderedBurger="addToOrder($event)"
              v-bind:key="burger.name"
              />
     

    </div>
    </section>

     

        <section id="info">
            <div class="burgerstyle">
                <h2>Customer information</h2>
                <p>Please fill this form to place your order</p> 
                <br>
                <form>
                    
                            
                    <p>
                        <label for="Fullname">Full name </label><br>
                        <input type="text" id="Fullname" v-model="fullName" required="required" placeholder="First- and Last name">
                    </p>
                    
                    
                    <p>
                        <label for="email">E-mail</label><br>
                        <input type="email" id="email" v-model="email" required="required" placeholder="E-mail address">
                    </p>
                    
                   
                   
                    <p>
                        <label for="payment">Payment method</label><br>
                        <select id="payment" v-model="payMethod">
                            <option disabled value=""> Payment method</option>
                            <option>Credit card</option>
                            <option>Swish</option>
                            <option>Cash</option>
                            <option>Bitcoin</option>
                        </select>
                        
                        
                    </p>
                    <h3>Please indicate point of delivery:</h3>
                <div id="smallMap">
        
        <div id="map" v-on:click="setLocation"  >
            <div id="dots">
                <img src="/img/polacks.jpg">
                
  <div 
       v-bind:style="{ left: location.x + 'px', 
                       top: location.y + 'px'}" > T </div>
</div>
        
        
        
        
       
</div>
    </div>
                    
                   <br>
                    <p>
                        <label for="Male">Male</label>
                        <input type="radio" id="Male" v-model="gender" value="Male" >
                    </p>
                    <p>
                        <label for="Female">Female</label>
                        <input type="radio" id="Female" v-model="gender" value="Female" >
                    </p>
                    <p>
                        <label for="idk">nonbinary</label>
                        <input type="radio" id="nonbinary" v-model="gender" value="Nonbinary" >
                    </p>
                 
          
                    

                    
                </form>
                
            </div> 
            
          </section>

 
  <br>
        
          <br>

         <Button @place-order="placeOrder" text="Place order" >
            
            
            </Button>    
           
    </main>
    <br>
    
    
   


    <footer>
        <hr>
        &copy; BurgerTownOnline
    </footer>
   </body>
      
</template>
<script>
import Burger from '../components/OneBurger.vue'
import menu from '../assets/menu.json'
import Button from '../components/Button.vue'
import io from 'socket.io-client'

const socket = io(); 


//Object Constructor Function
// The *this* keyword refers to the object itself


function MenuItem(pname, pic, energy, gluten, lactose, des1, des2) {
  this.name = pname;
  this.img = pic;
  this.kcal = energy;
  this.containsGluten = gluten;
  this.containsLactose = lactose;
  this.description1 = des1;
  this.description2 = des2;


}


let item1 = new MenuItem('The burger of death', 'https://media-cdn.tripadvisor.com/media/photo-s/13/8c/6d/99/dark-vader-burger.jpg', 700, true, true, "Nr 1 last meal of choice for death row inmates", "Will propably kill you");
let item2 = new MenuItem('The burger of famine', 'https://media.istockphoto.com/id/173619319/photo/begging.jpg?s=612x612&w=0&k=20&c=afgey5gPiTwRgiw51jM0ztSUADPcd1nFOKEIOz3tqk4=', 0, false, false, "Climate friendly: Do your part in saving the planet and starve", "May contain traces of suffering");
let item3 = new MenuItem('The burger of conquest', 'https://media.istockphoto.com/id/469660542/photo/favourite-burger-toppings.jpg?s=612x612&w=0&k=20&c=FAddvtpprWAuyIEeD7QxavR6CXD7pzkLHwRsWF01daY=', 2300, true, true, "With the infamous imperialist sauce", "May cointain traces of colonialism");
let item4 = new MenuItem('The burger of war', 'https://cdn.vox-cdn.com/thumbor/p_vvub1AqgZqpayyXQFNN9FqYVY=/28x0:472x333/1400x788/filters:focal(28x0:472x333):format(jpeg)/cdn.vox-cdn.com/uploads/chorus_image/image/39075092/black-bear-guinness-burger-eater.0.jpg', 12000, true, true, "Burger... burger never changes", "When all diplomatic options have failed");


let menuItems = [item1, item2, item3, item4];
console.log(menuItems);


export default {
  name: 'HomeView',
  components: {
    Burger,
    Button,
  },
  
  data: function () {
    return {
       yourVariable:"hej", 
       fullName:"",
        email:"",
        gender:"",
        payMethod:"",
        orderNumber:1,
      burgers: menu,
      orderedBurgers: {},
      orders: null,
      location: { x: 0,
            y: 0
          }
    }
  },
  created: function () {
    socket.on('currentQueue', data =>
      this.orders = data.orders);

  },
  methods: {
    addToOrder: function (event) {
  this.orderedBurgers[event.name] = event.amount;   

},

    placeOrder: function () {
        console.log(this.fullName, this.email, this.gender, this.payMethod, this.orderedBurgers, this.location.x, this.location.y);
        socket.emit("addOrder", { orderId: this.orderNumber,
                                details: this.location,
                                orderItems: this.orderedBurgers,
                                customer: {
                                  name: this.fullName,
                                  email: this.email,
                                  gender: this.gender,
                                  payMethod: this.payMethod
                                }


                                
                              }
                 );
                 this.orderNumber ++;

    },
    
    setLocation: function (event) {
        var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
                    this.location = { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y }
       

    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
                    this.location = { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }
}

</script>

<style>
body {
    background-color:blanchedalmond;
    font-family:"times new roman";
    font-size: 11pt;
    
}
section {
    margin:20px 0px 0px 10px;
    border: 2px dotted;
    border-radius: 0px;
    width: 100%;
    height: auto;
}
header {
    margin:0px 0px 0px 10px;
    overflow:hidden;
    width: 100%;
    height: 250px;
}
.keyword {
    text-transform: uppercase;

}
.important {
    text-transform: uppercase;
    font-style: italic;
    
}
.selection {
    color: white;
    background-color: #444;
}
#info{
    background-color:greenyellow;
    
}
button:hover {
    background-color:aqua;
    
    cursor:pointer;

 }
 button:active {
    transform: translateY(2px);


 }
 
 #banner {
    opacity: 1;
 }
 #headline {
    padding:20px;
    margin-top:-1440px;
    font-size: 3em;
    margin-left: 400px
    
    
 }
 .wrapper {
    display: grid;
    grid-template-columns: repeat(4,25%);
   

 }

 .burgerstyle{
    margin-left:20px;
    margin-bottom: 10px;
 }
 



  #map {
    width: 1920px;
    height: 1078px;
    
    
    
  }
  #smallMap {
    overflow: scroll;
    width: 1000px;
    height:400px;
  }
  
</style>