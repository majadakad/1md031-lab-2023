<template>
  <section id="min-header">
    <header>
      <h1 id="hamburgersida-titel">Välkommen till Majas hamburgare!</h1>
    </header>
  </section>

  <section id="tillgängligaHamburgare" class="tillgängliga-hamburgare">
    <header id="våra-hamburgare">
      <h2>Våra hamburgare</h2>
      <nav>Här väljer du burgare!</nav>
    </header>


    <div class = "wrapper">
      <Burger v-for="burger in burgers"
      v-bind:burger="burger" 
      v-bind:key="burger.name"
      v-on:orderedBurger="addToOrder($event)"/>
    </div>
</section>

  <section>
    <h2>
        Kundinformation
    </h2>

    <div>
        Här fyller du i dina kontakt- och beställningsuppgifter
        <h3>
            Beställningsuppgifter
        </h3>

        <div id="contact">
            <form>
                <input v-model="fn" placeholder="För- och efternamn" />

                <input type="radio" id="man" value="Man" v-model="gender" />
                <label for="man">Man</label>

                <input type="radio" id="woman" value="Woman" v-model="gender" />
                <label for="woman">Woman</label>

                <input type="radio" id="other" value="Other" v-model="gender" />
                <label for="other">Annat/Vill inte uppge</label>

                <input v-model="em" placeholder="Email-adress" />
            </form>
        </div>
      
  <section id="mapWrapper">
    Välj leveransadress: 
        <div id="map" v-on:click="setLocation">
        <div id="dots" v-bind:style="{ left: location.x + 'px', top: location.y + 'px' }">
        <div class="clickDot"> T</div>
        </div>
      </div>
</section>
    
    </div>
    <div>
        <h3>
            Betalningsmetod
        </h3>

        <p>
            <label for="Betalningsmetod">Betalningsmetod</label>
            <select id="Betalningsmetod" v-model="btm">
                <option>Klarna</option>
                <option>Mastercard</option>
                <option>Paypal</option>
                <option>Swish</option>
            </select>
        </p>
            
        <button class="lägg beställning" type="button" v-on:click = "orderButtonPressed"> 
          <img src="https://static.mathem.se/shared/images/products/large/kycklingburgare.jpg" 
          alt="Button Icon" 
          width="15" 
          height="15"> 
          Lägg beställning
        </button>
        {{ orderedBurgers }}
    </div>
</section>
    <section>
        <header>
            <h2>Majas hamburgare</h2>
        </header>

        <footer>
            End Notes
            &copy;
        </footer>
    </section>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import menu from '../assets/menu.json'
import io from 'socket.io-client'

const socket = io();

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  
  data() {
    return {
    fn: '',
    gender: '',
    em: '',
    lnName: '',
    lnNumber: '',
    burgers: menu,
    orderedBurgers: {},
    location: { x: 0,
            y: 0
          }
    };

  },
  
  methods: {
    
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },

    addToOrder: function (event) {
    this.orderedBurgers[event.name] = event.amount;
    },

    setLocation: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};

      this.location = {x: event.clientX - 10 - offset.x,
                       y: event.clientY - 10 - offset.y}
    },
    
    orderButtonPressed() {
    socket.emit("addOrder", { orderId: this.getOrderNumber(),
                              details: {x: this.location.x, y: this.location.y },
                              orderItems: Object.entries(this.orderedBurgers), 
                              fn: this.fn,
                              em: this.em,
                              gender: this.gender
                              }
                 );
    console.log('Order Items:', this.orderItems);

    },
  }
}
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Droid+Serif|Share+Tech+Mono");

body {
    font-family: 'Roboto Condensed', sans-serif;
    font-size: 2em;
}

section{
    margin: 0px;
    border-top: solid black;
    border-bottom: 0px;
    border-left: 0px; 
    border-right: 0px;
    border-width: 1.25em;
}

#min-header {
    background-image: url('https://media.istockphoto.com/id/614844110/sv/vektor/fast-food-seamless-pattern.jpg?s=612x612&w=0&k=20&c=bG62KCFmcHKSUuHJP5V6MnReI2v6AAI5cX23VGPcTnY=');
    height: 25em; 
}

#hamburgersida-titel{
    margin: 7em;
    color: black; 
    text-align: center;
}

.wrapper {
    display: grid;
    grid-gap: 3em;
    grid-template-columns: 11em 11em 11em;
    margin: 0em 0em 0em 2em;
}

button {
    width: 10em;
    height: 2em;
    font-size: 0.7em;
    text-align: left;
    margin: 0em 0em 1.7em 0em;
}

button:hover {
    background-color: rgb(78, 255, 47);
    cursor: pointer;
 }

#våra-hamburgare {
    margin: 0em 0em 0em 2em;
}

#tillgängligaHamburgare {
    color: white;
    background-color: black;
}

#mapWrapper {
  height: 23em;
  width: 10;
  overflow: scroll;
  border: solid black;
  border-width: 0.25em;

}

#map {
  background: url("/Users/majadakad/Documents/1md031-lab-2023/public/img/polacks.jpg");
  height: 1078px;
  width: 1920px;
  margin: 0em; 
}

#dots {
  position: absolute;
}

#dots div {
  position: absolute;
  background: black; 
  border-radius: 50%; 
  width: 10px; 
  height: 10px;
}
form input {
  width: 10em;
  height: 1.5em; 
  font-size: 0.7em;
}

form input[type = radio] {
  width: 0.7em;
  height: 0.7em; 
  font-size: 1em;
}
</style>