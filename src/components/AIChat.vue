<template>
  <div class="container" v-if="ready">
    <h1>AIChat</h1>
    <a-input :style="{width:'70%',height:'40px',margin:'30px 0'}" v-model="text" placeholder="什么都能问哦~"
             allow-clear/>
    <a-button :disabled="disableSend" type="primary" size="large" @click="handleSend"
              :style="{width:'150px'}">
      发送
    </a-button>
    <div style="width: 50%;max-height: 400px;overflow-x: hidden; overflow-y: scroll">
      <p v-for="item in msgList" :key="item.id" style="color: #7f7f7f">
        {{ '回答：' + item.msg }}
      </p>
    </div>
    <a-button type="outline" status="danger" size="large" @click="handleClear"
              :style="{position:'absolute', bottom:'20px', width:'150px',margin:'20px'}">
      退出
    </a-button>
  </div>
  <div class="container" v-if="!ready">
    <h1>AIChat</h1>
    <a-input :style="{width:'80%',height:'40px',margin:'30px 0'}" v-model="nick" placeholder="你叫什么" allow-clear/>
    <a-button type="primary" :loading="loading1" size="large" @click="handleNick"
              :style="{width:'150px',margin:'20px'}">
      确认
    </a-button>
  </div>
</template>

<script>
import axios from "axios";
import {Input} from '@arco-design/web-vue';
import {Button} from '@arco-design/web-vue';
import {Space} from '@arco-design/web-vue';

export default {
  name: 'AiChat',
  components: {
    AInput: Input,
    AButton: Button,
    ASpace: Space,
  },
  data() {
    return {
      text: undefined,
      ready: false,
      replay: undefined,
      loading: false,
      uid: undefined,
      nick: undefined,
      loading1: false,
      interval: undefined,
      msgList: [],
      disableSend: false
    }
  },

  created() {
    this.uid = sessionStorage.getItem("uid")
    this.ready = !!this.uid;
    if (this.ready) {
      this.interval = setInterval(this.getMsg, 4000);
    }
  },

  methods: {

    handleSend() {
      if (this.text && this.text.trim().length !== 0) {
        this.disableSend = true
        axios.get("http://120.26.227.16:8080/msg/save", {
          params: {text: this.text, from: this.uid},
        }).then(res => {

        }).finally(() => {
          this.disableSend = false
        })
      }
    },

    handleClear() {
      axios.get("http://120.26.227.16:8080/auth/logout", {
        params: {uid: this.uid}
      })
      clearInterval(this.interval)
      sessionStorage.clear()
      this.ready = false
      this.nick = undefined
    },

    handleNick() {
      if (this.nick && this.nick.trim().length !== 0) {
        this.loading1 = true
        axios.get("http://120.26.227.16:8080/auth/login", {}).then(res => {
          if (res.data.code === 200) {
            sessionStorage.setItem("uid", res.data.data)
            this.uid = res.data.data
            this.$message.info(res.data.msg)
            this.ready = true
          } else {
            this.$message.error(res.data.msg)
          }
        }).finally(() => {
          this.loading1 = false
        })
      }
    },

    getMsg() {
      this.loading1 = true
      axios.get("http://120.26.227.16:8080/msg/list", {
        params: {uid: this.uid}
      }).then(res => {
        this.msgList = res.data.data
      }).finally(() => {
        this.loading1 = false
      })
    }
  },
  beforeDestroy() {
    clearInterval(this.interval);
  },

  destroyed() {
    this.handleClear()
  }
}
</script>

<style scoped>
.container {
  width: 100vw;
  height: calc(100vh - 20%);
  overflow: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10% 0;
}

h1 {
  margin: 0;
  font-size: 4rem;
  font-family: "Times New Roman", serif;
}

</style>
