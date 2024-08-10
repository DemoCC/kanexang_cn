<template>
  <div class="container">
    <!--    <chat-index></chat-index>-->
    <ai-chat></ai-chat>
  </div>
</template>
<script>
import dogIndex from '@/components/DogIndex.vue'
import chatIndex from '@/components/ChatIndex.vue'
import AiChat from '@/components/AIChat.vue'

export default {
  components: {
    AiChat,
    dogIndex,
    chatIndex,
  },
  data() {
    return {}
  },

  created() {
    document.title = 'AIChat'
    document.oncontextmenu = function () {
      // return false
    };
    document.onkeydown = function (e) {
      if (e.code === 'F12') {
        // return false
      }
    }
  },

  methods: {}
}
</script>

<style scoped>
</style>
