<template>
  <div>
    <header>
      <h3>{{ burger.name }}</h3>
    </header>

    <nav>{{ burger.description }}</nav>

    <img :src="burger.img" :alt="burger.name" :style="{ width: '300px', height: '250px' }">

    <div class="ingredienser_titel">
      Innehåll:
      <ul class="ingredienser_innehållslista">
        <li v-for="ingredient in burger.ingredients" :key="ingredient">{{ ingredient }}</li>
      </ul>
    </div>
   Lägg till i beställning:  {{ amountOrdered }}
   <button @click = "addBurger"> + </button>
   <button @click = "removeBurger"> - </button>

  </div>
  </template>
  
  <script>
  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },

    data() {
    return {
      amountOrdered: 0,
    }
    },
    
    methods: {
    addBurger: function () {
      this.amountOrdered += 1;
      this.$emit('orderedBurger', {name:this.burger.name, 
                                  amount: this.amountOrdered 
                                  }
                );
    }, 
    removeBurger: function() {
      this.amountOrdered -= 1;
      this.$emit('orderedBurger', {name:this.burger.name, 
                                  amount: this.amountOrdered 
                                  }
                );
    }
  }
}
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

@import url("https://fonts.googleapis.com/css?family=Droid+Serif|Share+Tech+Mono");

template {
  font-family: 'Roboto Condensed', sans-serif;
  font-size: 2em;
}

div{
  margin: 0px;
  border-top: solid black;
  border-bottom: 0px;
  border-left: 0px; 
  border-right: 0px;
  border-width: 1.25em;
}


.ingredienser_titel{
    font-size: 1em;
    color: rgba(169, 155, 112, 0.616);
}

.ingredienser_innehållslista{
    font-weight: bold;
    font-size: 0.7em;
    color: rgb(255, 255, 255);
}
  </style>
  