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
        <label class="tab_item" for="all" @click="tabButton">繰り返し</label>
        <input id="programming" type="radio" name="tab_item">
        <label class="tab_item" for="programming" @click="tabButton">1回のみ</label>
        <div class="tab_content" id="all_content">
          <div class="tab_content_description">
            <h2>間隔</h2>
            <select v-model="interval" @change="selectChange" name="selectInterval">
              <option value="毎日">毎日</option>
              <option value="毎週">毎週</option>
              <option value="隔週">隔週</option>
              <option value="毎月">毎月</option>
              <option value="毎年">毎年</option>
            </select>
            <div class="display_none" id="every_week">
              <p>毎週</p>
            </div>
            <div class="display_none" id="every_other_week">
              <p>隔週</p>
            </div>
            <div class="display_none" id="every_month">
              <p>毎月</p>
            </div>
            <div class="display_none" id="every_year">
              <p>毎年</p>
            </div>
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
            <input type="date" v-model="date">
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
      date: '',
      command: ''
    }
  },
  methods: {
    tabButton: function () {
      this.interval = ''
      this.date = ''
    },
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
      this.command = '/remind ' + this.address + ' "' + this.message + '" at ' + this.hour + ':' + this.minutue + this.morningAfternoon + ' ' + this.interval + this.date
    },
    // リストで選択された値に応じて要素を表示する
    selectChange: function () {
      if (this.interval === '毎日') {
        this.displayHide()
      } else if (this.interval === '毎週') {
        this.displayChange('#every_week')
      } else if (this.interval === '隔週') {
        this.displayChange('#every_other_week')
      } else if (this.interval === '毎月') {
        this.displayChange('#every_month')
      } else if (this.interval === '毎年') {
        this.displayChange('#every_year')
      }
    },
    // 引数に指定された要素を表示する
    displayChange: function (id) {
      this.displayHide()

      var el = document.querySelector(id)
      el.classList.remove('display_none')
      el.classList.add('display_show')
    },
    // 既にリストで値が選択され表示されている要素が存在する場合、非表示にする
    displayHide: function () {
      if (document.querySelector('.display_show')) {
        // display_showクラスを持つ要素のidを取得し#idの形にする
        var displayShowId = '#' + document.querySelector('.display_show').getAttribute('id')
        var displayShowElement = document.querySelector(displayShowId)
        displayShowElement.classList.remove('display_show')
        displayShowElement.classList.add('display_none')
      }
    }
  },
  computed: {
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

.display_show {
  display: block;
}

.display_none {
  display: none;
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
