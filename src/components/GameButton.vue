<template>
    <button :class="['game-button', color.name, { lit: color.isLit }]" @click="emitClick" :index="index"></button>
</template>
  
<script>
  export default {
    name: 'GameButton',
    props: ['color', 'lit', 'index'],

    methods: {
      emitClick() {
        this.$emit('click', this.index);
    }
    }
  };
</script>
  
<style scoped>
.game-button {
    width: 100px;
    height: 100px;
    margin: 10px;
}

.red { 
  background-color: red;
}
.green {
  background-color: green;
}
.blue {
  background-color: blue;
}
.yellow {
  background-color: yellow;
}
.lit { 
    box-shadow: 0 0 15px 5px rgba(255, 255, 255, 0.75); 
    transform: scale(1.1); 
}
</style>



  