<template>
  <div class="wrapper">
    <img src="../assets/present.png" width="100" alt="">
    <h1> откроется через <br>{{ hours }}:{{ minutes }}:{{seconds}}</h1>
  </div>
</template>

<script>
  const dayjs = require('dayjs')
  export default {
    name: "WtfView",

    data() {
      return {
        hours: this.format(23 - dayjs().hour()),
        minutes:   this.format(59 - dayjs().minute()),
        seconds: this.format(59 - dayjs().second()),
      }
    },

    created() {
      const interval = setInterval(() => {
        this.hours =   this.format(23 - dayjs().hour());
        this.minutes = this.format(59 - dayjs().minute());
        this.seconds = this.format(59 - dayjs().second());
        console.log(dayjs().date())
        if(dayjs().date() >= 12) {
          clearInterval(interval)
          this.$router.push('/')
        }
      }, 1000)
    },

    methods: {
      format(num) {
        num = JSON.stringify(num);
        return num.length > 1 ? num : '0' + num;
      }
    },

    mounted() {

    }
  }
</script>

<style scoped>

  @keyframes wiggle {
    from {
      transform: translate(0, -5px);
    }

    50% {
      transform: translate(0, 5px);
    }

    to {
      transform: translate(0, -5px);
    }
  }


  
  .wrapper {
    padding:  30px;
    display: flex; 
    justify-content: center;
    text-align: center; 
    flex-direction: column;
    align-items: center;
    height:  100vh;
  }

  h1 {
    line-height: 1em;
    font-family: beb;
    font-size: 70px;
    font-weight: 100;
  }

  img {
    animation:  wiggle 1s;
    animation-iteration-count: infinite;
    width: 15vw;
  }

  @media screen and (max-width:  600px){
    h1 {
      font-size: 40px;
    }

    img {
      width: 20vw;
    }
  }
</style>