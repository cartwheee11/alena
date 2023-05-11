<template>
  <div class="wrapper">
    <div class="text-wrapper">
      <div class="text-container">
        <p>С Днём рождения! 🎂🎂🎂<br>Небольшой подгон приедет 13-14 мая на Гагарина 21А</p>
        <h1>{{ code }}</h1>
        <p>назови там этот код товара ^_^</p>    
      </div>
    </div>
    
    <div class="images-wrapper">
      <img src="../assets/1.png" alt="" width="70">
      <img src="../assets/2.png" alt="" width="50">
      <img src="../assets/3.png" alt="" width="60">
      <img src="../assets/4.png" alt="" width="50">
      <img src="../assets/5.png" alt="" width="70">
      <img src="../assets/6.png" alt="" width="50">
      <img src="../assets/7.png" alt="" width="50">
      <img src="../assets/8.png" alt="" width="100">
      <img src="../assets/9.png" alt="" width="50">
      <img src="../assets/10.png" alt="" width="50">
      <img src="../assets/11.png" alt="" width="40">
      <img src="../assets/12.png" alt="" width="30">
      <img src="../assets/heart.png" alt="" width="60">
      <img src="../assets/heart.png" alt="" width="80">
      <img src="../assets/heart.png" alt="" width="50">
      <img src="../assets/heart.png" alt="" width="90">
    </div>  
  </div>
  
</template>

<script>
  
  export default {
    name: 'CongratsView',
    data() {
      return {
        code: localStorage.getItem('code'),
        images: [],
        vel: []
      }
    },

    mounted() {
      const imgs = document.querySelectorAll('img');
      this.images = imgs;

      [].forEach.call(imgs, img => {
        this.vel.push(Math.random() * 3 + 1)
        img.style = `
          transform: rotate(${Math.random() * 180}deg);
          top: ${Math.random() * window.innerHeight - 50}px;
          left: ${Math.random() * window.innerWidth - 50}px;
        `
      })

      this.startUpdating();
    },

    methods: {
      startUpdating() {

        let update = () => {
          [].forEach.call(this.images, (img, i) => {
            img.style.top = parseInt(img.style.top) > window.innerHeight ? - 100 + 'px' : parseInt(img.style.top) + this.vel[i] + 'px'
          })  



          setTimeout(update.bind(this), 16)
        }
        

        update();
      }
    }
  }
</script>

<style scoped>
  @keyframes fade-in {
    from {
      opacity: 0;
    }

    30% {
      opacity: 0;
    }

    to {
      opacity: 1; 
    }
  }

  .wrapper {
    animation:  fade-in 3s;
  }

  .images-wrapper {
    overflow-y: hidden !important;
    overflow-x: hidden !important;
    height: 100vh;
    width: 100vw;
    position: fixed;
    top:  0;
    left: 0;
    z-index: 0;
  }


  .text-wrapper {
    text-align: center;
    display: flex;
    position: fixed;
    flex-direction: column;
    align-content: center;
    align-items: center;
    justify-content: center;
    height: 100vh;
    width:  100%;
    z-index: 1;
    box-shadow: 0px 0px 95px 42px #fff;
  }

  .text-container {
    display: flex;
    align-content: center;
    flex-direction: column;
    align-items: center;
    background-color: #fff;
    box-shadow: 0px 0px 95px 100px rgba(255,255,255,1)
  }

  img {
    position: absolute;
  }

  p {
    margin: 0;
    vertical-align: middle;
    width:  70%;
  }

  h1 {
    margin: 0;
    vertical-align: middle;
    font-size: 16vw;
    font-family: 'beb';
    line-height: 1em;
    margin-top: 20px;
  }

  @media screen and (max-width: 600px) {
    h1 {
      margin: 0;
      vertical-align: middle;
      font-size: 22vw;
      font-family: 'beb';
      line-height: 1em;
      margin-top: 20px;
    }
  }
</style>