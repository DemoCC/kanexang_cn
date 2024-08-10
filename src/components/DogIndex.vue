<template>
  <div class="container" ref="container"
       :style="{position:' absolute', width: '100%',height: '100%',backgroundColor:bgColor}">
    <h3>{{ words }}</h3>

    <!--  换背景  -->
    <svg t="1703237415215" @click="changeBackground" class="icon icon-change-bg" viewBox="0 0 1024 1024" version="1.1"
         xmlns="http://www.w3.org/2000/svg"
         p-id="7570" width="25" height="25">
      <path
          d="M128.299 128C92.788 128 64 156.788 64 192.299v639.4C64 867.212 92.788 896 128.299 896H895.7c35.512 0 64.3-28.788 64.3-64.299V192.299C960 156.788 931.212 128 895.701 128H128.299zM128 588.313l178.162-178.162c24.795-24.795 64.996-24.795 89.792 0L817.803 832H128V588.313z m768 231.375L666.385 590.073l64.718-64.718c25.153-25.152 65.933-25.152 91.085 0L896 599.167v220.521zM694.65 471.299l-73.519 73.519L432.653 356.34c-45.064-45.064-118.127-45.064-163.19 0L128 497.803V192h768v316.657l-37.358-37.358c-45.286-45.285-118.707-45.285-163.992 0z"
          p-id="7571" fill="#cdcdcd"></path>
    </svg>

    <!--  换词条  -->
    <svg t="1703233253688" @click="getWords" class="icon icon-change-word" viewBox="0 0 1024 1024" version="1.1"
         xmlns="http://www.w3.org/2000/svg"
         p-id="1479"
         width="25" height="25">
      <path
          d="M512.034377 0a512 512 0 0 1 296.813714 929.206857l-87.113142-117.540571a365.714286 365.714286 0 1 0-426.569143-5.12l84.041143-109.348572L512.034377 1024H128.034377l77.970286-101.595429-10.971429-8.338285A512 512 0 0 1 512.034377 0z"
          fill="#cdcdcd" p-id="1480"></path>
    </svg>
  </div>
</template>

<script>
import data from '../data/data.json'

export default {
  name: 'dogIndex',
  data() {
    return {
      words: undefined,
      bgColor: "#282828"
    }
  },

  created() {
    //禁止debug
    document.oncontextmenu = function () {
      return false
    };
    document.onkeydown = function (e) {
      if (e.code === 'F12') {
        return false
      }
    }
    this.getWords()
  },
  methods: {
    getWords() {
      const index = Math.floor(Math.random() * data.length);
      this.words = data[index].data;
    },

    changeBackground() {

    }
  }
}
</script>

<style scoped>
h3 {
  display: block;
  width: 50%;
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(100px) saturate(150%);
  color: rgba(24, 24, 24, 0.5);
  margin: 100px auto;
  border-radius: 20px;
  padding: 20px;
  line-height: 2;
  text-indent: 2em;
  box-shadow: 0px 4px 5px 0px #0f0e0e4d
}

.icon-change-bg {
  left: 10%;
}

.icon-change-word {
  right: 10%;
}

.icon:hover {
  cursor: pointer;
  background: rgba(0, 0, 0, 0.2);
  transition: 0.3s linear;
}

.icon {
  position: fixed;
  bottom: 30px;
  padding: 8px;
  border-radius: 10px;
  background: rgba(255, 255, 255, 0.2);
  backdrop-filter: blur(100px) saturate(150%);
}
</style>
