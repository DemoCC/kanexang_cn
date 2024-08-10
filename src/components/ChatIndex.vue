<template>
  <div class="container">
    <div class="top">
      <div>
        <img src="../assets/chat/ljl/ljl1.gif" alt="" class="ljl1">
      </div>
      <input v-model="text" :class="inputShake" placeholder="请向实力波波提问..." @keyup.enter="send"/>
      <img @click="send" src="../assets/chat/send.svg" class="head" alt="实力波波">
    </div>


    <div class="bottom">
      <div class="loading" v-show="loading">
        <svg t="1705562709130" class="icon-loading" viewBox="0 0 1024 1024" version="1.1"
             xmlns="http://www.w3.org/2000/svg"
             p-id="7138" width="20" height="20">
          <path
              d="M475.428571 97.52381h73.142858v219.428571h-73.142858z m0 609.523809h73.142858v219.428571h-73.142858zM926.47619 475.428571v73.142858h-219.428571v-73.142858z m-609.523809 0v73.142858H97.52381v-73.142858zM779.215238 193.072762l51.712 51.687619-155.136 155.184762-51.736381-51.736381zM348.208762 624.054857l51.736381 51.736381-155.160381 155.136-51.712-51.687619zM193.097143 244.784762l51.687619-51.712 155.184762 155.136-51.736381 51.736381z m430.982095 431.006476l51.736381-51.736381 155.136 155.160381-51.687619 51.712z"
              p-id="7139" fill="#bfbfbf"></path>
        </svg>
        <p style="color: silver; font-size: 13px">请稍等...</p>
      </div>
      <p class="replay-text">{{ replay }}</p>
    </div>

    <video class="video" ref="video" src="../assets/chat/ljl/ljl.mp4"></video>

    <span @click="playLjl" ref="btn-replay"
          style="position: fixed; left: 10px; bottom: 10px; z-index: 999; color: white; display: none">重播</span>
    <span @click="closeVideo" ref="btn-close"
          style="position: fixed; right: 10px; bottom: 10px; z-index: 999;color: white; display: none">关闭</span>

  </div>
</template>

<script>
import http from 'axios'

export default {
  name: 'aiIndex',
  data() {
    return {
      text: undefined,
      replay: undefined,
      inputShake: "",
      loading: false
    }
  },

  methods: {
    async send() {
      if (this.text === undefined || this.text.trim() === "") {
        this.inputShake = 'input-shake'
        await new Promise((resolve) => setTimeout(resolve, 1000))
        this.inputShake = ''
      } else if (this.text.includes("李佳乐")) {
        this.playLjl()
        return true
      } else {
        this.loading = true
        http.get("https://api.a20safe.com/api.php", {
          params: {
            api: 51,
            key: "eac190d8b34c0e57443b77b44deed248",
            text: this.text
          }
        }).then(res => {
          if (res.status === 200) {
            this.replay = res.data.data[0].reply
          }
          this.loading = false
        })
      }
    },

    playLjl() {
      const dom = this.$refs['video']
      const bnt1 = this.$refs['btn-replay']
      const btn2 = this.$refs['btn-close']
      dom.style.display = 'block'
      bnt1.style.display = 'block'
      btn2.style.display = 'block'
      dom.play()
    },

    closeVideo() {
      const dom = this.$refs['video']
      const bnt1 = this.$refs['btn-replay']
      const btn2 = this.$refs['btn-close']
      dom.style.display = 'none'
      bnt1.style.display = 'none'
      btn2.style.display = 'none'
    }
  }
}
</script>

<style scoped>
.container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
}

.top, .bottom {
  width: 97%;
  text-align: center;
}

.top {
  padding-top: 70px;
}

.bottom {
  height: 70%;
}

input {
  width: 60%;
  height: 50px;
  outline: none;
  border: none;
  border-radius: 7px;
  background-color: rgba(181, 178, 178, 0.1);
  text-align: center;
  font-size: 18px;
  margin-top: 30px;
  color: #9c9c9c;
}

input::placeholder {
  color: rgb(217, 217, 217);
}

@keyframes typing {
  from {
    width: 0;
  }
  to {
    width: 100%;
  }
}

/**
input震动
 */
@keyframes shake {
  0% {
    transform: translate(1px, 1px) rotate(0deg);
  }
  10% {
    transform: translate(-1px, -2px) rotate(-1deg);
  }
  20% {
    transform: translate(-3px, 0px) rotate(1deg);
  }
  30% {
    transform: translate(3px, 2px) rotate(0deg);
  }
  40% {
    transform: translate(1px, -1px) rotate(1deg);
  }
  50% {
    transform: translate(-1px, 2px) rotate(-1deg);
  }
  60% {
    transform: translate(-3px, 1px) rotate(0deg);
  }
  70% {
    transform: translate(3px, 1px) rotate(-1deg);
  }
  80% {
    transform: translate(-1px, -1px) rotate(1deg);
  }
  90% {
    transform: translate(1px, 2px) rotate(0deg);
  }
  100% {
    transform: translate(1px, -2px) rotate(0deg);
  }
}

/**
input震动
 */
.input-shake {
  animation: shake 0.80s cubic-bezier(.36, .07, .19, .97) both;
  transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
  perspective: 1000px;
}

.replay-text {
  color: silver;
  font-size: 17px;
  width: 60%;
  margin: 20px auto;
  line-height: 2;
}

.chip {
  display: block;
  width: 60px;
  height: 60px;
  margin: 0 auto;
}

.head {
  width: 45px;
  height: 45px;
  vertical-align: middle;
  margin-left: 13px;
  cursor: pointer;
  transition: 0.2s;
}

.icon {
  vertical-align: middle;
  margin-left: 20px;
}

.head:hover {
  width: 40px;
  height: 40px;
}

.icon-loading {
  margin-top: 20px;
  animation: rotation 1s infinite linear;
  transform-origin: center;
}

@keyframes rotation {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(359deg);
  }
}

.ljl1 {
  width: 100px;
  height: 120px;
  animation: move1 1s infinite linear;
}

@keyframes move1 {
  50% {
    transform: translateX(250px);
  }
}


.video {
  display: none;
  position: fixed;
  width: 100%;
}
</style>
