<template>
  <div ref="wrapper" class="wrapper">
    <div class="avatar-container" style="text-align: center; margin-top: 20px; margin-bottom: 20px">
      <img src="../assets/avatar.png" style="border-radius:100%; width:70px;" alt="">
      <p style="color: #555; font-size: 16px; padding: 0; margin: 0;">Ванюшка</p>
    </div>
    <div class="messages-field">
      <div  v-for="message in messages" :key="message" class="message" :class='{"incoming": message.type === "incoming", "outgoing": message.type === "outgoing"}'>

        <div v-if="!message.clickable" class="message-text-container">{{message.text}}</div>
        <a style="white-space: pre-wrap" v-else-if="message.clickable" target=”_blank” href="https://music.vk.com/promocode" class="message-text-container message-clickable">{{message.text}}</a>

        <div class="clear"></div>
      </div>
    </div>

    <div class="white-space" style="height: 100px"></div>

    <div class="input-container">
      <input @keypress.enter="onSendButtonClick"  v-model="input" placeholder="Сообщение" type="text">
      <button @click="onSendButtonClick"></button>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
const dayjs = require('dayjs');
const cryptojs = require('crypto-js');
import CongratsView from './CongratsView.vue'

export default {
  name: 'HomeView',
  data() {
    return {
      input: '',
      components: { CongratsView },
      encrypted: 'U2FsdGVkX1+x+FKw65PMEqhQ5RGxBG2oKAECLImYHLk=',
      messages: [
        { type: 'incoming', text: `Привет, Алёнк 😈 У меня для тебя кое-что есть 🤔` },
        { type: 'incoming', text: "Но чтобы я понял, что это реально ты, реши небольшую задачку: введи кличку своей коши + второе слово твоей третьей сохранёнки с конца (ебейший квест)" }
      ],
      congrats: false,
      decrypted: null
    }
  },

  beforeMount() {
    if(dayjs().date() < 12) {
      this.$router.push('/wtf')
    }
  },

  methods: {
    send(type, text, clickable = false) {


      if(clickable) {
        this.messages = this.messages.concat([{type, text, clickable: true}])
        return;
      } 

      this.messages = this.messages.concat([{type, text}])
      if(text === '/подсказка') {
        this.$nextTick(() => {
          window.scrollTo(0, document.body.scrollHeight)
        })
        return
      }

      if(type === 'outgoing') {
        let decrypted = cryptojs.AES.decrypt(this.encrypted, text.toLowerCase().replaceAll(' ', '')).toString(
          cryptojs.enc.Utf8
        );

        if(decrypted) {
          setTimeout(() => {
            this.congrats = true;
            this.decrypted = decrypted;
            localStorage.setItem('code', decrypted)
            this.$refs.wrapper.classList.add('fade-out')

            this.send('incoming', 'Верно! Сек...')

            setTimeout(() => {
              this.$router.push({ path: '/congrats' })  
            }, 3000)
            
          }, 1000)
        } else {
          setTimeout(() => {
            this.send('incoming', 'Нихуя! Походу это не ты :(')
            this.send('incoming', 'Чтобы заюзать подсказку, введи /подсказка')
          }, 1000)
        }
      }

      this.$nextTick(() => {
        window.scrollTo(0, document.body.scrollHeight)
      })
    },

    onSendButtonClick() {
      if (this.input === '') return;
      this.send('outgoing', this.input)
      if(this.input === '/подсказка') {
        setTimeout(() => {
          this.send('incoming', "Хуле тут подсказывать, всё и так понятно) Не, ну если совсем не получается, то напиши в вк пхпх")
        }, 1000)
      }
      this.input = '';
    }
  }
}
</script>

<style scoped>
  @keyframes show {
    from {
      transform: translate(0, 20px);
      opacity: 0;
    }

    to {
      transform: translate(0, 0);
      opacity: 1; 
    }
  }

  @keyframes fadeoutе {
    from {
      opacity: 1;
    } 

    30% {
      opacity: 1;
    }

    to {
      opacity: 0;
    }
  }

  .fade-out {
    animation: fadeoutе 3s;
  }

  .wrapper {
    max-width: 600px;
    margin: 0 auto;
    /*padding: 10px;*/
  }


  .messages-field {
    padding:  10px;
  }
  

  .input-container {
    display:  flex;
    background-color: #fff;
    padding:  10px 0;
    width:  100%;
    max-width: 600px;
    position: fixed;
    bottom: 0px;
    padding:  10px;
  }

  .message {
    animation: show 0.2s;
    animation-fill-mode: forwards;
  }

  .message-text-container {
    padding: 15px 20px;
    border-radius: 30px;
    margin-bottom:  10px;
    background-color: #3781f5;
    max-width:  80%;
    overflow-wrap: break-word;
    color:  white;
    display: inline-block;
    word-wrap: wrap;
  }

  .outgoing .message-text-container {
    float:  right;
    background-color: #ebebf3;
    color:  #444;
  }

  .input-container input {
    flex-grow: 1;
  }

  .message-clickable {
    background-color: #8241eb;
  }

  @media screen and (max-width:  600px) {
    div, input {
      font-size: 16px;
    }
  }
</style>
