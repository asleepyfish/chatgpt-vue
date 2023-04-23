<template>
  <div class="home">
    <input v-model="content" type="text">
    <button @click="startStreaming" style="margin-left: 20px">提问</button>
    <br><br>
    <textarea v-model="chatContent" rows="30" cols="100"></textarea>
  </div>
</template>

<script>

export default {
  name: 'HomeView',
  data() {
    return {
      content: '',
      chatContent: ''
    };
  },


  methods: {
    startStreaming() {
      // 创建 XMLHttpRequest 对象
      const xhr = new XMLHttpRequest();
      // 设置请求的 URL
      xhr.open('GET', `http://localhost:8080/streamChatWithWeb?content=${this.content}`);
      // 设置响应类型为 text/event-stream
      xhr.setRequestHeader('Content-Type', 'text/event-stream');
      // 监听 readyStateChange 事件
      xhr.onreadystatechange = () => {
        // 如果 readyState 是 3，表示正在接收数据
        if (xhr.readyState === 3) {
          // 将数据添加到文本框中
          this.chatContent = xhr.responseText;
        }
      };
      // 发送请求
      xhr.send();
    }
  }
}
</script>
