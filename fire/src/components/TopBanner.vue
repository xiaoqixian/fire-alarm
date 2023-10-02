
<script>
import {ref} from 'vue';
const zh_week = ["", "一", "二", "三", "四", "五", "六", "日"];

export default {
  data: function() {
    const _date = new Date();
    return {
      time: ref(_date.getHours() + ":" + _date.getMinutes()),
      date: ref(_date.getFullYear() + "/" + _date.getMonth() + "/" + _date.getDate()),
      weekday: ref("星期" + zh_week[_date.getDay()]),
      temperature: ref('32°C'),
      PM_value: ref('PM 2.5')
    }
  },
  mounted: function() {
    this.timer = setInterval(() => {
      const _date = new Date();
      this.time = _date.getHours() + ":" + _date.getMinutes();
    }, 60000);

    this.date_timer = setInterval(() => {
      const _date = new Date();
      this.date = _date.getFullYear() + "/" + _date.getMonth() + "/" + _date.getDate();
      this.weekday = "星期" + zh_week[_date.getDay()];
    }, 1440000);

  }
}

</script>

<template>
  <div id="top-banner">
    <div class="left">
      <div id="clock">
        <div id="time">{{ time }}</div>
        <div id="date"> {{ date }} | {{ weekday }}</div>
      </div>
      <img id="weather-img" src="../assets/smallrain.png"/>
      <div id="weather">
        <div id="temperature">{{ temperature }}</div>
        <div id="pm">{{ PM_value }}</div>
      </div>
    </div>
    <div class="mid">
      智慧安消防一体化监管系统
    </div>
    <div class="right">
      <img src="../assets/notifications.png"/>
      <img src="../assets/toggle.png"/>
      <img src="../assets/default_avatar.png"/>
    </div>
  </div>
</template>

<style>
#top-banner {
  display: flex;
  height: 70px;
  flex: initial;
  width: 100%;
}
#top-banner > .left {
  display: flex;
  flex: initial;
}
#clock {
  margin-right: 10px;
}
#time {
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
  text-align: center;
}
#date {
  font-size: 15px;
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
}
#weather-img {
    height: 80%;
}
#weather {
  flex: flex-start;
}
#temperature {
  font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
  padding-bottom: 3px; 
}
#pm {
  font-size: 15px;
  font-family: Arial, Helvetica, sans-serif;
  border-top: 1px solid white;
}

#top-banner > .mid {
  flex: auto;
  font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
  font-size: 30px;
  text-align: center;
}

#top-banner > .right {
  display: flex;
  flex: initial;
  height: 50%;
  width: 150px;
  justify-content: flex-end;
  align-items: center;
}
#top-banner .right img {
  flex: initial;
  width: 50%;
  height: 50%;
  object-fit: contain;
}
</style>
