<template>
  <header>
        <img src="https://beyondtype1.org/wp-content/uploads/2023/01/FAST-FOOD-CHAIN-NUTRITION-GUIDE-HEADER-1200x429.jpg" alt="fast food header">

        <section class="head_text">
            <h1>
                Welcome to BurgerOnline!
            </h1>
            <h2>
                The biggest, and only, selection of online burgers
            </h2>
        </section>
        
    </header>
    
    
    <main>
        <section id="burger_info">
            <section id="burger_info_header">
                <h3>Burger selection</h3>
                <p>Please select the burgers you wish to order</p>
            </section>
            
            <div class="burger_wrapper">      
              <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"
              v-on:orderedBurger="addToOrder($event)"/>  
            </div>

        </section>

        <section class="delivery_info">
            <h3>Delivery inforamtion</h3>
            <p>Please insert the requested information</p>
            <p>
                <label for="name">Full name</label><br>
                
                <input type="text" id="name" v-model="name" required="required" placeholder="First- and last name">
            </p>
            <p>
                <label for="email">Email</label><br>
                <input type="email" id="email" v-model="mail" placeholder="email">
            </p>
            <p>
                <label for="payment">Payment method</label><br>
                <select id="payment" v-model="pay">
                    <option selected="selected">Card</option>
                    <option>Cash upon delivery</option>
                    <option>Billed to adress</option>
                    <option>PayPal</option>
                    

                </select>
            </p>
            <form>
                <label for="female">Female</label>
                <input type="radio" id="female" v-model="gender" name="gender" value="female">
                <br />
                <label for="male">Male</label>
                <input type="radio" id="male" v-model="gender" name="gender" value="Male">
                <br />
                <label for="other">Other/do not wish to specify</label>
                <input type="radio" id="other" name="gender" v-model="gender" checked="checked" value="other">
            </form>

            <p>Pick a location for delivery on the map below</p>
            <div id="mapwrap">
      <img id="map" v-on:click="setLocation" src="../../public/img/polacks.jpg" >
      <div  v-bind:style="{ left: this.location.x + 'px', top: this.location.y + 'px'}">
          T
          </div>
      </div>
        </section>
        
        

        <button type="submit" v-on:click="sendInfo">
            <img src="https://cdn2.iconfinder.com/data/icons/shopping-e-commerce-2-1/32/Success-Place-Order-Complete-Shopping-Tick-512.png" style="width: 30px;">
            Place order
        </button>
    </main>

    <footer>
        <hr>
        Have a nice day!   
    </footer>

</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'


const socket = io();

/*function MenuItem(product, url, energy, allergies, details) {
  this.name = product;
  this.img = url;
  this.kCal = energy;
  this.allergies = allergies
  this.description = details;
}*/

// created as const because we might want to change the menu but it will always be and array :)
/*const burgerArray = [new MenuItem('BB-Budget burger', 'https://s7d1.scene7.com/is/image/mcdonalds/Header_Cheeseburger_832x472:1-3-product-tile-desktop?wid=763&hei=472&dpr=off', 300, ['lactose', 'gluten'],'Our most budget friedly burger, very basic' ), 
                    new MenuItem('FB-Fisht burger', 'https://s7d1.scene7.com/is/image/mcdonalds/mcd-sv-products-burgers-filet-o-fish-NEW:1-3-product-tile-desktop?wid=829&hei=515&dpr=off', 250, ['none'],'A pesciterain friendly burger, straight from the ocean'),
                    new MenuItem('KB-King Burger','https://s7d1.scene7.com/is/image/mcdonalds/mcd-sv-products-burgers-big-tasty-thickerbeef:1-3-product-tile-desktop?wid=829&hei=515&dpr=off',500,['gluten'],"It's big, it's tasty, it's fit for a KING")];
*/

export default {
  name: 'HomeView',
  components: {
    Burger,
},
  data: function () {
    return {
      burgers: menu,
      orderedBurgers:{},
      location: { x: 0,
            y: 0
          }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    sendInfo: function(){
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: this.location,
                                orderItems: this.orderedBurgers,
                                customerInformation: {name: this.name, mail: this.mail, pay: this.pay, gender: this.gender}
                              }
                 );

    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    setLocation: function (event) {
      this.location = {x: event.clientX - 10 - event.currentTarget.getBoundingClientRect().left,
                    y:  event.clientY - 10 - event.currentTarget.getBoundingClientRect().top};
    }
  }
}
</script>

<style>
/* comment */

@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';

body {
    font-family:'Helvetica Neue', sans-serif;
    font-size: 15pt;
}

header {
    margin-left: 10px;
    height: 250px;
    overflow: hidden;

}

.head_text{
    top: 10vw;  /* Where it is based on the parent (in this case header) */
    left: 10vw;
    position: absolute;

}

header img{
    opacity: 0.55;
    object-fit: cover;
    width: 100%;
    height: auto;
}

.burger_wrapper {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: 33% 33% 33%;
}


#burger_info {
    margin: 10px 0;
    color: white;
    background-color: black;
    border: dashed white;
    
}

#burger_info_header{
    padding: 10px;
    
}

#burger_info div{
    margin: 10px;
    padding: 10px;
}

#allergy{
    font-weight: bold;
}

.delivery_info {
    border: 2px dashed black;
    padding-left: 10px;
    margin-bottom: 10px;
}

button[type="submit"]:hover  {
    background-color: green;
    cursor: grab;
}

button[type="submit"]{
    margin-top: 10px;
    width: 20vw;
    font-size: 20px;
}
  #mapwrap{
    overflow-x: scroll;
    position: relative;
  }

  #mapwrap div{
    position: absolute;
    background-color: black;
    border-radius: 100%;
    width: 20px;
    height: 20px;
    color: white;
    text-align:center ;
  }

</style>