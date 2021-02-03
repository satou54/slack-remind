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
      <input type="text" id="symbol" v-model="symbol" readonly>
      <input type="text" id="address" v-bind:placeholder="target" v-model="address">
    </div>
    <div>
      <h2>メッセージ</h2>
      <input type="text" id="message" placeholder="内容" v-model="message">
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
            <select v-model="interval">
              <option value="everyday">毎日</option>
              <option value="毎週">毎週</option>
              <option value="隔週">隔週</option>
              <option value="毎月">毎月</option>
              <option value="毎年">毎年</option>
            </select>
            <h2>時間</h2>
            <select v-model="morningAfternoon">
              <option value="am">午前</option>
              <option value="pm">午後</option>
            </select>
            <input v-model="hour" id="hour" placeholder="00時">
            <input v-model="minutue" id="minute" placeholder="00分">
          </div>
        </div>
        <div class="tab_content" id="programming_content">
          <div class="tab_content_description">
            <h2>間隔</h2>
            <input type="date">
            <h2>時間</h2>
            <select v-model="morningAfternoon">
              <option value="am">午前</option>
              <option value="pm">午後</option>
            </select>
            <input v-model="hour" placeholder="00時">
            <input v-model="minutue" placeholder="00分">
          </div>
        </div>
      </div>
    </div>
    <button id="create" @click="createButton">生成</button>
    <div>
      <h3>Slack command</h3>
      <input id="slack_command" placeholder="Slack command" v-bind:value="command">
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      symbol: '',
      target: '',
      address: '',
      message: '',
      interval: '',
      morningAfternoon: '',
      hour: '',
      minutue: '',
      command: ''
    }
  },
  methods: {
    channelButton: function () {
      this.symbol = '#'
      this.target = 'channel'
      this.address = ''
    },
    someoneButton: function () {
      this.symbol = '@'
      this.target = 'someone'
      this.address = ''
    },
    meButton: function () {
      this.symbol = '@'
      this.address = 'me'
    },
    createButton: function () {
      this.command = '/remind ' + this.address + ' "' + this.message + '" at ' + this.hour + ':' + this.minutue + this.morningAfternoon + ' ' + this.interval
    }
  },
  computed: {
    addressTest: function () {
      return this.address
    },
    messageTest: function () {
      return this.message
    },
    selectedRemind: function () {
      return this.interval
    },
    selectedMorningAfternoon: function () {
      return this.morningAfternoon
    },
    hourTest: function () {
      return this.hour
    },
    minutueTest: function () {
      return this.minutue
    }
  }
}
</script>

<style scoped>
.container {
  border: 2px solid #000;
  margin: 50px;
}

#symbol {
  width: 15px;
  background: #d4d4d4;
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

#hour {
  width: 50px;
}

#minute {
  width: 50px;
}

#create {
  margin-top: 20px;
}

#slack_command {
  margin-bottom: 30px;
  width: 400px;
  height: 30px;
}
</style>
