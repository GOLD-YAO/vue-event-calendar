<template>
  <div class="events-wrapper" >
    <div class="coachstab">
        <div class="coachhas"></div>
        <div class="coachhas"></div>
    </div>
    <div   class="coachsstyle">
      <span class="coachi"></span>点击日历即可查看教练当日安排
    </div>
    <!-- <h2 class="date">
      {{dayEventsTitle}}
    </h2> -->
    <div class="cal-events">
      <slot>
        <div v-for="(event, index) in events" class="event-item" :key="index">
          <cal-event-item :event="event" :index="index" :locale="locale"></cal-event-item>
        </div>
      </slot>
    </div>
  </div>
</template>

<script>
import i18n from '../i18n.js'
import { dateTimeFormatter } from '../tools.js'
import calEventItem from './cal-event-item.vue'
export default {
  name: 'cal-events',
  data () {
    return {
      i18n
    }
  },
  components: {
    'cal-event-item': calEventItem
  },
  props: {
    title: String,
    dayEvents: {
      type: Object,
      required: true
    },
    locale: {
      type: String,
      required: true
    },
    color: {
      type: String,
      required: true
    }
  },
  computed: {
    dayEventsTitle () {
      if (this.title) return this.title
      if (this.dayEvents.date !== 'all') {
        let tempDate
        if (this.dayEvents.events.length !== 0) {
          tempDate = Date.parse(new Date(this.dayEvents.events[0].date))
          return dateTimeFormatter(tempDate, i18n[this.locale].fullFormat)
        } else {
          tempDate = dateTimeFormatter(Date.parse(new Date(this.dayEvents.date)), i18n[this.locale].fullFormat)
          return `${tempDate} ${i18n[this.locale].notHaveEvents}`
        }
      } else {
        return i18n[this.locale].dayEventsTitle
      }
    },
    events () {
      return this.dayEvents.events
    },
    bgColor () {
      return {backgroundColor: this.color}
    }
  },
  methods: {
    dateTimeFormatter
  }
}
</script>


<style lang="less">
    .coachsstyle{
      height:50px;
      text-align: center;
      line-height: 50px;
      color:  #ccc;
      font-size: 18px;
      position: relative;
      .coachi{
        display:inline-block;
        width: 20px;
        height: 20px;
        background: #4ead8d;
        border-radius: 50%;
        position: absolute;
         top: 28%;
    margin-left: -26px;


      }
    }
    .coachstab{
      height: 50px;
      position:relative;
      border-top: 2px solid #ccc;
      .coachhas{
        border-style: solid;
        // border-width: 15px 15px 15px 15px;
        // border-color: red forestgreen blue cyan;
        border-width: 0px 30px 20px 0px;
        border-color: transparent #44b549 transparent transparent !important;
        width: 30px;
        height: 20px;
        position: absolute;  
        top:60%;
        left:50px;
        margin-top:-15px;
        &:after{
          content: '有安排';
          height: 20px;
          width: 50px;
          line-height: 20px;
          position: absolute;
          left: 40px;
        }
        &:nth-child(2){
          border-width: 0px 30px 20px 0px;
          border-color: transparent #ea6977 transparent transparent !important;
          left:150px;
          &:after{
             content: '请假';
          }
        }
      }
    
      
    }
</style>
