<template>
  <section>
    <header id="pagetop">
    <h1 id="pagetitle">VWelcome to Burgershop24 </h1>
        <img id="titlepic" src="/Users/edvinmartna/1MD031-lab-2024/public/img/wallpap.jpg" style="width: 1350px">
        <h2> Choose your burgers below: </h2>
    </header>
    </section>
    <main>
        <section id="orders">
            <div class="Burgertypes">
              <div>
    <h1>Burgers</h1>
    <div class="Burgertypes">
    <Burger v-for="burger in burgers"
            v-bind:burger="burger" 
            v-bind:key="burger.name"
            v-on:orderedBurger="addToOrder($event)"/>
    </div>
    
</div>
              <!--
            <div class="burger a">
            <h3>Burger</h3>
            <img src="/Users/edvinmartna/1MD031-lab-2024/public/img/burger1.png" style="width: 350px">
                <ul>
                    <li>Contains <span class="allergens"> gluten </span> and <span class="allergens"> meat </span> of some sort</li>
                    <li>Good flavour, so I've heard</li>
                    <li>Not eligible for discounts</li>
                </ul>
            </div>
            <div class="burger b">
            <h3>Cheezy surprise</h3>
            <img src="/Users/edvinmartna/1MD031-lab-2024/public/img/burger2.JPG" style="width: 350px">
                <ul>
                    <li>Contains <span class="allergens"> gluten </span> and <span class="allergens"> meat </span> of some sort</li>
                    <li>Taste like cheese with burger</li>
                    <li>Not eligible for discounts</li>
                </ul>
            </div>
            <div class="burger c">
            <h3>Frutti Di Mare</h3>
            <img src="/Users/edvinmartna/1MD031-lab-2024/public/img/burger3.jpg" style="width: 350px">
                <ul>
                    <li>Contains <span class="allergens"> gluten </span> and catch of the day (<span class="allergens"> fish </span>)</li>
                    <li>May contain <span class="allergens"> mercury </span></li>
                    <li>Not eligible for discounts</li>
                </ul>
            </div>
            <div class="burger d">
            <h3>Vegan delight</h3>
            <img src="/Users/edvinmartna/1MD031-lab-2024/public/img/burger 4.png" style="width: 350px">
                <ul>
                    <li>Cruelty-free (vegan)</li>
                    <li>Fiber over flavour</li>
                    <li>Not eligible for discounts</li>
                </ul>
            </div>
          -->
            </div>
        </section>
        <section>
            <section id="contact">
                <h2> Customer Information </h2>
                <form action= "" onsubmit="event.preventDefault();">
                    <p>
                        <label for="name">Full name</label><br>
                        <input type="text" id="name" v-model="form.fn" required="required" placeholder="First- and last name">
                    </p>
                    <p>
                        <label for="email">Email</label><br>
                        <input type="email" id="email" v-model="form.em" required="required" placeholder="Email address">
                    </p>
                    <p>
                        <label for="Payment">Payment method</label>
                        <select id="Payment" v-model="form.rcp">
                            <option selected="selected">Card</option>
                            <option>Bank Account</option>
                            <option>Swish</option> 
                            <option>Wire Transfer</option>
                            <option>Mail us cash in a box</option>
                        </select>
                    </p>
                    <p id=form-prompt>Gender</p>
                    <input type="radio" id="Male" v-model="form.gender" value="Male">
                    <label for="Male">Male</label><br>
                    <input type="radio" id="Female" v-model="form.gender" value="Female">
                    <label for="Female">Female</label><br>
                    <input type="radio" id="NIL" v-model="form.gender" value="NIL">
                    <label for="NIL">Prefer not to say</label><br>
                    <button v-on:click="markDone(key)" type="submit">
                    <label> Save details </label>
                    </button>
                </form>
                
            </section>
        </section>
        <h2> Delivery location </h2>
        <div id="mapwrap">
        <div id="map" v-on:click="setLocation">
          <div id="dots">
          <div v-bind:style="{ left: this.location.x + 'px', top: this.location.y + 'px'}">
            T
          </div>
      </div>
      </div>
    </div>
    <button v-on:click="addOrder" type="submit">
      Place order
    </button>
    </main>
    <footer>
        <hr>
        End notes
    </footer>


</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '/Users/edvinmartna/1MD031-lab-2024/src/assets/menu.json'

const socket = io("localhost:3000");
function MenuItem(name, cals, picture, gluten, meat, info) {
    this.name = name; // The *this* keyword refers to the object itself
    this.cals = cals;
    this.picture = picture;
    this.gluten = gluten;
    this.lactose = meat;
    this.info = info
}
const burger1 = new MenuItem(
  'Burger',
   '300',
    'public/img/burger1.png',
     'yes',
      'yes',
    ['Contains gluten and meat of some sort',
                    'Good flavour',
                    'Not eligible for discounts']);
const burger2 = new MenuItem(
  'Cheezy surprise',
   '300',
   'public/img/burger2.JPG',
    'yes',
     'yes',
     ['Contains gluten and cheese and meat of some sort',
     'Taste like cheese with burger',
     'Not eligible for discounts']);
const burger3 = new MenuItem('Frutti Di Mare',
 '300', 
 'public/img/burger3.jpg',
  'yes',
   'no',
   ['Contains gluten and catch of the day ( fish )',
   'May contain mercury',
   'Not eligible for discounts']);
const burger4 = new MenuItem('Vegan Delight',
 '300',
 'public/img/burger 4.png',
  'no',
   'no',
   ['Cruelty-free (vegan)',
   'Fiber over flavour',
   'Not eligible for discounts']);
var burgersarr = [burger1, burger2, burger3, burger4];
export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      form: {
        fn: '',
        em: '',
        rcp: '',
        gender: ''
      },
      detailsAreSaved: false,
      orderedBurgers: {},
      location: { x: 0,
            y: 0
          }
    }
  },
  methods: {
    setLocation: function(event){
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top};
      this.location.x = event.clientX - 10 - offset.x;
      this.location.y = event.clientY - 10 - offset.y;
    },
    addToOrder: function(event){
      this.orderedBurgers[event.name] = event.amount;
      if (event.amount === 0){
       delete this.orderedBurgers[event.name];
      }
    },

    markDone: function(key) {
      console.log(this.form);
      this.detailsAreSaved = true;

  },
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {

        if (Object.keys(this.orderedBurgers).length === 0) {
          console.log("empty basket");
          return;
        }
        if (!this.detailsAreSaved) {
          console.log("no personal data :(");
          return;
        }
        
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: this.location.x - 15,
                                           y: this.location.y - 25 },
                                orderItems: this.orderedBurgers,
                                personalInfo: { name: this.form.fn,
                                email: this.form.em,
                                gender: this.form.gender,
                                payment: this.form.rcp,
                                }
                              }
                 );                 
    }
  }
}
</script>

<style>
@import 'https://fonts.googleapis.com/css?family=Playfair|Display';
body {
   font-family: Playfair;
}
#pagetop {
    overflow: hidden;

}
#pagetitle {
    position: absolute;
    color: White;
    scale: 300%;
    padding: 82px 0px 0px 255px;
}
#titlepic {
    width: 100%;
    height: auto;
}
.allergens {
   color: #ff5500;
}

#orders div {
    margin: 30px 15px 15px 15px;
    padding: 5px 5px 5px 5px;
}
#orders {
   background-color: black;
   color: white;
   border: 10px dotted white;
}
body > section {
    margin: 30px 15px 15px 30px;
}
main > section {
    margin: 30px 15px 15px 30px;
}

.Burgertypes {
  display: grid;
     grid-gap: 10px;
     grid-template-columns: 500px 500px;
     background-color: black;
     color: black;
}








#contact {
    background-color: gray;
    color: black;
    border: 10px dotted black;
    padding: 20px 20px 20px 20px;
}
button {
    margin: 30px 0px 0px 0px;
}
button:hover {
   background-color: lightblue;
   cursor: pointer;
}
  #map {
    background: url("/img/polacks.jpg");
    width: 1920px;
    height: 1078px;
  }
  #mapwrap {
    overflow: scroll;
    width: 600px;
    height: 600px;
  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-repeat: no-repeat;
    width:1920px;
    height: 1078px;
    cursor: crosshair;
    background-image: url('/img/polacks.jpg');
  }
</style>