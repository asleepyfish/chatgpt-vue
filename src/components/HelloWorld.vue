<template>
  <div class="hello">
    <el-row>
      <el-col :span="12">
        <span class="beautiful">流式问答</span>
        <el-card class="box-card">
          <div slot="header" class="clearfix">
            <el-row>
              <el-col :span="18">
                <el-input v-model="content" placeholder="请输入内容"></el-input>
              </el-col>
              <el-col :span="6">
                <el-button @click="startStreaming">提问</el-button>
              </el-col>
            </el-row>

          </div>
          <div class="text item">
            <markdown-it-vue-light :content="chatContent"/>
          </div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import MarkdownItVueLight from 'markdown-it-vue/dist/markdown-it-vue-light.umd.min.js'
import 'markdown-it-vue/dist/markdown-it-vue-light.css'

export default {
  name: 'HomeView',
  data() {
    return {
      content: '',
      chatContent: ''
    };
  },

  components: {
    MarkdownItVueLight
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.text {
  font-size: 14px;
  text-align: left;
}

.item {
  margin-bottom: 18px;
  text-align: left;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}

.clearfix:after {
  clear: both
}

.box-card {
  width: 100vw;
}

span.beautiful {
  display: inline-block;
  padding: 10px;
  background-color: #f0f0f0;
  color: #333333;
  border-radius: 5px;
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 10px;
}
</style>
