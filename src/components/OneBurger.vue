<template>
    <div style="display: inline-block;" class="burgers">
  <h5>{{ burger.name }}</h5>
  <img v-bind:src="burger.img" style="width: 20vw;">

  <section class="details">
      <ul>
          <li>{{ burger.kCal }} kCal</li>
          <li>{{burger.description}}</li>
          <li v-if="burger.allergies.length > 0"> 
            Contains:
            <span  id="allergy" >
              {{getAllergies}}
            </span>
          </li>
      </ul>
  </section>
  <section class="add">
    <button type="button" v-on:click="subOne">-</button>
    {{ amountOrdered }}
    <button type="button" v-on:click="addOne">+</button>
  </section>
  
</div>
  </template>
  
  <script>

  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },
    data: function () {
    return {
      amountOrdered: 0,
      }
    },
    methods:{
      addOne: function(){
        this.amountOrdered ++
        this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
        );
      },
      subOne: function(){
        if(this.amountOrdered > 0){
          this.amountOrdered --
          this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              }
          );
        }
      }
    },
    computed: {
    getAllergies: function (){
      return this.burger.allergies.join(" and ")
      }
    } 
  }

  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>

  .burgers{
    position: relative;
  }
  
  #burger_info {
  color: white;
  background-color: black;
  
}

#burger_info_header{
  padding: 10px;
}

#burger_info div{
  margin: 10px;
  padding: 10px;
  border: 2px dashed white;
}

#allergy{
  font-weight: bold;
}

.add{
  display: inline-block;
  position: absolute;
  transform: translate(-50%, 0);
  left: 50%;
  bottom: 0;
  font-size: 24px;
}

.add button{
  font-size: 18px;
}


  
</style>


  