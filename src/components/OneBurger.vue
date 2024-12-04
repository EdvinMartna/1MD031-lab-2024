<template>
    <div class="burger">
            <h3>{{ burger.name }}</h3>
            <img v-bind:src="burger.url" style="width: 350px">
                <ul>
                    <li>{{ burger.info[0] }}</li>
                    <li>{{ burger.info[1] }}</li>
                    <li>{{ burger.info[2] }}</li>
                    <div class="allergies">
    <ul>
      <p>
        <span v-if= "burger.meat > false">
    Contains meat
  </span>
      </p>
      <p><span v-if= "burger.gluten >false">
    Contains gluten
  </span></p>

    </ul>
    
  </div>
                </ul>
                <button v-on:click="addBurger()" type="button"> 
                  <label> Add this to order </label>
                </button>
                <button v-on:click="removeBurger()" type="button"> 
                  <label> Remove this from order </label>
                </button>
                In cart {{amountOrdered}}
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
methods: {
  addBurger: function () {
  this.amountOrdered += 1;
  this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              } 
  );
},
removeBurger: function () {
  if (this.amountOrdered < 1) {
    this.amountOrdered +=1
  }
  this.amountOrdered -= 1;
  this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              } 
  );
},
}
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 .burger {
     color: white;
     border-radius: 5px;
     font-size: 150%;
 }
 .allergies {
  color:darkgoldenrod
 }


</style>