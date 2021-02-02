<template>
  <div class="container">
    <h1>Slack remind自動生成サイト</h1>
    <p>Slackのremindコマンドを生成することができます。</p>
    <div>
      <h2>宛先</h2>
      <button @click="channelButton">#channel</button>
      <button @click="someoneButton">@someone</button>
      <button @click="meButton">me</button>
      <br>
      <input type="text" id="address" placeholder="対象" v-model="address">
      <p>{{ addressTest }}</p>
    </div>
    <div>
      <h2>メッセージ</h2>
      <input type="text" id="message" placeholder="内容" v-model="message">
      <p>{{ messageTest }}</p>
    </div>
    <div>
        <h2>リマインド</h2>
      <div class="tabs">
        <input id="all" type="radio" name="tab_item" checked>
        <label class="tab_item" for="all">繰り返し</label>
        <input id="programming" type="radio" name="tab_item">
        <label class="tab_item" for="programming">1回のみ</label>
        <div class="tab_content" id="all_content">
          <div class="tab_content_description">
            <h2>間隔</h2>
            <select>
              <option value="#">毎日</option>
              <option value="#">毎週</option>
              <option value="#">隔週</option>
              <option value="#">毎月</option>
              <option value="#">毎年</option>
            </select>
            <h2>時間</h2>
            <select>
              <option value="#">午前</option>
              <option value="#">午後</option>
            </select>
            <input placeholder="00時">
            <input placeholder="00分">
          </div>
        </div>
        <div class="tab_content" id="programming_content">
          <div class="tab_content_description">
            <h2>間隔</h2>
            <select>
              <option value="#">毎日</option>
              <option value="#">毎週</option>
              <option value="#">隔週</option>
              <option value="#">毎月</option>
              <option value="#">毎年</option>
            </select>
            <h2>時間</h2>
            <select>
              <option value="#">午前</option>
              <option value="#">午後</option>
            </select>
            <input placeholder="00時">
            <input placeholder="00分">
          </div>
        </div>
      </div>
    </div>
    <button id="create">生成</button>
    <div>
      <h3>Slack command</h3>
      <input id="slack_command" placeholder="Slack command">
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      address: '',
      message: ''
    }
  },
  methods: {
    channelButton: function () {
      this.address = '#channel'
    },
    someoneButton: function () {
      this.address = '@someone'
    },
    meButton: function () {
      this.address = '@me'
    }
  },
  computed: {
    addressTest: function () {
      return this.address
    },
    messageTest: function () {
      return this.message
    }
  }
}
</script>

<style scoped>
.container {
  border: 2px solid #000;
  margin: 50px;
}

#address {
  margin-top: 10px;
}

/*タブ切り替え全体のスタイル*/
.tabs {
  margin-top: 50px;
  padding-bottom: 40px;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  width: 700px;
  margin: 0 auto;}

/*タブのスタイル*/
.tab_item {
  width: calc(100%/3);
  height: 50px;
  border-bottom: 3px solid #5ab4bd;
  background-color: #d9d9d9;
  line-height: 50px;
  font-size: 16px;
  text-align: center;
  color: #565656;
  display: block;
  float: left;
  text-align: center;
  font-weight: bold;
  transition: all 0.2s ease;
}
.tab_item:hover {
  opacity: 0.75;
}

/*ラジオボタンを全て消す*/
input[name="tab_item"] {
  display: none;
}

/*タブ切り替えの中身のスタイル*/
.tab_content {
  display: none;
  padding: 40px 40px 0;
  clear: both;
  overflow: hidden;
}

/*選択されているタブのコンテンツのみを表示*/
#all:checked ~ #all_content,
#programming:checked ~ #programming_content,
#design:checked ~ #design_content {
  display: block;
}

/*選択されているタブのスタイルを変える*/
.tabs input:checked + .tab_item {
  background-color: #5ab4bd;
  color: #fff;
}

#create {
  margin-top: 20px;
}

#slack_command {
  margin-bottom: 30px;
}
</style>
