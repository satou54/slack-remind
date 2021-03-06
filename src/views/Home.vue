<template>
  <div class="container mx-auto">
    <div class="row">
      <div class="col-10 mx-auto">
        <h1>Slack remind自動生成サイト</h1>
        <p>Slackのremindコマンドを生成することができます。</p>
      </div>
    </div>
    <div class="row">
      <div class="co-10 mx-auto">
        <h2 class="mt-4">宛先</h2>
        <button class="btn btn-primary mr-1" @click="channelButton">#channel</button>
        <button class="btn btn-primary mr-1" @click="someoneButton">@someone</button>
        <button class="btn btn-primary mr-1" @click="meButton">me</button>
        <br>
        <div class="col-10 my-1 mt-3 mx-auto">
          <label class="sr-only" for="inlineFormInputGroupUsername">宛先</label>
          <div class="input-group">
            <div class="input-group-prepend">
              <div class="input-group-text pr-3 pl-1" id="symbol">{{ symbol }}</div>
            </div>
            <input type="text" class="form-control" id="address" v-bind:placeholder="target" v-model="address">
          </div>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-7 mt-4 mx-auto">
        <h2>メッセージ</h2>
        <textarea class="form-control" id="message" rows="3" placeholder="内容" v-model="message"></textarea>
      </div>
    </div>
    <div class="row">
      <div class="col-10 mt-4 mx-auto">
        <h2>リマインド</h2>
        <div class="tabs col-10">
          <input id="all" type="radio" name="tab_item" checked>
          <label class="tab_item" for="all" @click="tabButton">繰り返し</label>
          <input id="programming" type="radio" name="tab_item">
          <label class="tab_item" for="programming" @click="tabButton">1回のみ</label>
          <div class="tab_content" id="all_content">
            <div class="tab_content_description">
              <h2 class="mb-2">間隔</h2>
              <select v-model="selectInterval" @change="selectChange" name="selectInterval" class="form-control col-3 mx-auto">
                <option value="毎日">毎日</option>
                <option value="毎週">毎週</option>
                <option value="隔週">隔週</option>
                <option value="毎月">毎月</option>
                <option value="毎年">毎年</option>
              </select>
              <div class="display_none from-group mt-2" id="every_week">
                <div class="mt-3">
                  <button class="btn btn-primary mr-1" @click="setOrdinaryDays">平日</button>
                  <button class="btn btn-primary" @click="setHolidaty">土日</button>
                </div>
                <div v-for="(weekOfDay, i) in weekOfDays" :key="i" class="weekOfDay mt-3 mr-2">
                  <input
                    :id="'weekOfDay' + i"
                    :name="'weekOfDay' + i"
                    type="checkbox"
                    :value="weekOfDay"
                    v-model="selectedWeekOfDays"
                  >
                  <label :for="'weekOfDay' + i">{{ weekOfDay }}</label>
                </div>
              </div>
              <div class="display_none" id="every_other_week">
                <div v-for="(weekOfDay, i) in weekOfDays" :key="i" class="weekOfDay form-check form-check-inline mt-3">
                  <input
                    class="form-check-input"
                    :id="'weekOfDayOption' + i"
                    name="weekOfDayOption"
                    type="radio"
                    :value="weekOfDay"
                    v-model="selectedWeekOfDay"
                  >
                  <label class="form-check-label" :for="'weekOfDayOption' + i">{{ weekOfDay }}</label>
                </div>
              </div>
              <div class="display_none mt-3" id="every_month">
                <div class="form-inline">
                  <div class="mx-auto">
                    <input v-model="day" class="form-control col-4 text-center" placeholder="Day">
                    <p class="col-1 pl-1" style="display:inline;">日</p>
                  </div>
                </div>
              </div>
              <div class="display_none mt-3" id="every_year">
                <div class="form-inline">
                  <div class="mx-auto">
                    <input v-model="month" class="form-control col-4 text-center" placeholder="Month"><p class="col-1 pl-1" style="display:inline;">月</p>
                    <input v-model="day" class="form-control col-4 text-center" placeholder="Day"><p class="col-1 pl-1" style="display:inline;">日</p>
                  </div>
                </div>
              </div>
              <h2 class="mt-4">時間</h2>
              <div class="form-inline">
                <div class="mx-auto">
                  <select v-model="morningAfternoon" class="form-control col-3 mr-1">
                    <option class="form-control" value="am">午前</option>
                    <option class="form-control" value="pm">午後</option>
                  </select>
                  <input v-model="hour" class="form-control col-3 mr-1 text-center" id="repeat-hour" placeholder="00時">
                  <input v-model="minutue" class="form-control col-3 text-center" id="repeat-mimute" placeholder="00分">
                </div>
              </div>
            </div>
          </div>
          <div class="tab_content" id="programming_content">
            <div class="tab_content_description">
              <h2>日付</h2>
              <input type="date" v-model="date" @change="changeDate" class="form-control col-6 mx-auto text-center">
              <h2 class="mt-4">時間</h2>
              <div class="form-inline">
                <div class="mx-auto">
                  <select v-model="morningAfternoon" class="form-control col-3 mr-1">
                    <option value="am">午前</option>
                    <option value="pm">午後</option>
                  </select>
                  <input v-model="hour" class="form-control col-3 mr-1 text-center" id="once-hour" placeholder="00時">
                  <input v-model="minutue" class="form-control col-3 mr-1 text-center" id="once-mimute" placeholder="00分">
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <button class="btn btn-primary mt-4" id="create" @click="createButton">生成</button>
    <div class="row">
      <div class="col-10 mt-4 mb-5 mx-auto">
        <h3>Slack command</h3>
        <div class="form-inline">
          <div class="mx-auto">
            <input class="form-control col-8" id="slack_command" placeholder="Slack command" v-bind:value="command">
            <button class="btn ml-1" @click="copyToClipboard">copy</button>
          </div>
        </div>
      </div>
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
      selectInterval: '',
      interval: '',
      ordinaryDaysFlg: false,
      holidayFlg: false,
      weekOfDays: ['月', '火', '水', '木', '金', '土', '日'],
      selectedWeekOfDays: [],
      selectedWeekOfDay: '',
      day: '',
      month: '',
      morningAfternoon: '',
      hour: '',
      minutue: '',
      date: '',
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
    tabButton: function () {
      this.interval = ''
      this.date = ''
    },
    createButton: function () {
      this.command = this.createSlackCommand()
    },
    createSlackCommand: function () {
      var command

      switch (this.interval) {
        case 'every weekday':
          if (this.selectedWeekOfDays.length === 7) {
            command = 'on every weekday'
          } else {
            command = 'on every'
            for (var i = 0; i < this.selectedWeekOfDays.length; i++) {
              command += ' ' + this.weekOfDaysReplace(this.selectedWeekOfDays[i]) + ','
            }
            command.slice(0, -1)
          }
          break
        case 'every other':
          command = 'every other ' + this.weekOfDaysReplace(this.selectedWeekOfDay)
          break
        case 'every month':
          command = 'on ' + this.day + ' every month'
          break
        case 'every year':
          if (this.month.length === 1) {
            this.month = 0 + this.month
          }
          if (this.day.length === 1) {
            this.day = 0 + this.day
          }
          command = 'on ' + this.month + '-' + this.day + ' every year'
          break
        case 'once':
          command = 'on ' + this.date
          break
      }
      if (this.hour.length === 1) {
        this.hour = 0 + this.hour
      }
      if (this.minutue.length === 1) {
        this.minutue = 0 + this.minutue
      }

      return '/remind ' + this.address + ' "' + this.message + '" at ' + this.hour + ':' + this.minutue + this.morningAfternoon + ' ' + command
    },
    weekOfDaysReplace: function (weekOfDay) {
      switch (weekOfDay) {
        case '月':
          return 'Monday'
        case '火':
          return 'Tuesday'
        case '水':
          return 'Wednesday'
        case '木':
          return 'Thursday'
        case '金':
          return 'Friday'
        case '土':
          return 'Saturday'
        case '日':
          return 'Sunday'
      }
    },
    // リストで選択された値に応じて要素を表示する
    selectChange: function () {
      this.selectedWeekOfDays = []
      if (this.selectInterval === '毎日') {
        this.displayHide()
        this.interval = 'everyday'
      } else if (this.selectInterval === '毎週') {
        this.displayChange('#every_week')
        this.interval = 'every weekday'
      } else if (this.selectInterval === '隔週') {
        this.displayChange('#every_other_week')
        this.interval = 'every other'
      } else if (this.selectInterval === '毎月') {
        this.displayChange('#every_month')
        this.interval = 'every month'
      } else if (this.selectInterval === '毎年') {
        this.displayChange('#every_year')
        this.interval = 'every year'
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
    },
    setOrdinaryDays: function () {
      const monday = document.getElementById('weekOfDay0')
      const tuesday = document.getElementById('weekOfDay1')
      const wednesday = document.getElementById('weekOfDay2')
      const thursday = document.getElementById('weekOfDay3')
      const friday = document.getElementById('weekOfDay4')
      var ordinaryDays = ['月', '火', '水', '木', '金']

      if (!this.ordinaryDaysFlg) {
        this.ordinaryDaysFlg = true
        monday.checked = true
        tuesday.checked = true
        wednesday.checked = true
        thursday.checked = true
        friday.checked = true
        Array.prototype.push.apply(this.selectedWeekOfDays, ordinaryDays)
      } else {
        this.ordinaryDaysFlg = false
        monday.checked = false
        tuesday.checked = false
        wednesday.checked = false
        thursday.checked = false
        friday.checked = false
        this.selectedWeekOfDays = this.remodeArray(this.selectedWeekOfDays, ['土', '日'])
      }
    },
    setHolidaty: function () {
      const saturday = document.getElementById('weekOfDay5')
      const sunday = document.getElementById('weekOfDay6')
      var holiday = ['土', '日']

      if (!this.holidayFlg) {
        this.holidayFlg = true
        saturday.checked = true
        sunday.checked = true
        Array.prototype.push.apply(this.selectedWeekOfDays, holiday)
      } else {
        this.holidayFlg = false
        saturday.checked = false
        sunday.checked = false
        this.selectedWeekOfDays = this.remodeArray(this.selectedWeekOfDays, ['月', '火', '水', '木', '金'])
      }
    },
    remodeArray: function (array, removeArray) {
      var newArray = []
      array = array.filter(function (value) {
        for (var i = 0; i < removeArray.length; i++) {
          if (value === removeArray[i]) {
            newArray.push(value)
          }
        }
      })
      return newArray
    },
    changeDate: function () {
      this.interval = 'once'
    },
    copyToClipboard: function () {
      var copyTarget = document.getElementById('slack_command')
      copyTarget.select()
      document.execCommand('Copy')
    }
  },
  computed: {
  }
}
</script>

<style scoped>
body {
  background: #340c34;
}

.container {
  border: 2px solid #5ab4bd;
  margin: 50px;
}

#symbol {
  width: 15px;
  background: #d4d4d4;
}

.btn {
  background-color: #5ab4bd;
  color: #fff;
  border: none;
}

.btn:hover {
  background-color: #5ab4bd;
  opacity: 0.75;
}

/*タブ切り替え全体のスタイル*/
.tabs {
  margin-top: 50px;
  padding-bottom: 40px;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  width: auto;
  margin: 0 auto;}

/*タブのスタイル*/
.tab_item {
  width: calc(100%/5);
  height: 40px;
  background-color: #d9d9d9;
  border-bottom: 3px solid #5ab4bd;
  line-height: 47px;
  font-size: 16px;
  text-align: center;
  color: #565656;
  display: block;
  float: left;
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
  padding: 20px 40px 0;
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
  margin-bottom: 10px;
  margin-top: 10px;
  width: 400px;
  height: 30px;
}

.weekOfDay {
  display: inline-block;
}
</style>
