<template>
    <div class="schedule-config">
      <div>
        <span class="cron-ico">
          <i class="el-icon-date" size="small"></i>
          <span class="character" @click="scheduleEdit">SCHEDULER</span>
        </span>
        <el-switch :disabled="!schedule.cronExpression" v-model="schedule.enable" @change="scheduleChange"/>
        <ms-schedule-edit :schedule="schedule" :save="save" ref="scheduleEdit"/>
        <crontab-result v-show="false" :ex="schedule.cronExpression" ref="crontabResult" @resultListChange="recentListChange"/>
      </div>
      <div>
        <span :class="{'disable-character': !schedule.enable}"> 下次执行时间：{{this.recentList.length > 0 ? this.recentList[0] : ''}} </span>
      </div>
    </div>
</template>

<script>
    import MsScheduleEdit from "./MsScheduleEdit";
    import CrontabResult from "../cron/CrontabResult";
    export default {
      name: "MsScheduleConfig",
      components: {CrontabResult, MsScheduleEdit},
      data() {
        return {
          recentList: [],
        }
      },
      props: {
        save: Function,
        schedule: {},
        checkOpen: {
          type: Function,
          default() {
            return new Function()
          }
        },
      },
      // mounted() {
      //   console.log(this.schedule);
      //   // this.recentList = this.$refs.crontabResult.resultList;
      //   // console.log(this.recentList);
      //   console.log(this.recentList+ "====");
      // },
      // watch: {
      //   'schedule.cronExpression'() {
      //     console.log(this.schedule);
      //     this.$refs.crontabResult.expressionChange();
      //     this.recentList = this.$refs.crontabResult.resultList;
      //     console.log(this.recentList);
      //   }
      // },
      methods: {
        scheduleEdit() {
          if (!this.checkOpen()) {
            return;
          }
          this.$refs.scheduleEdit.open();
        },
        scheduleChange() {
          this.$emit('scheduleChange');
        },
        recentListChange(resultList) {
          this.recentList = resultList;
        }
      }
    }
</script>

<style scoped>

  .schedule-config {
    float: right;
    width: 250px;
    height: 15px;
    line-height: 25px;
  }

  .el-icon-date {
    font-size: 20px;
    margin-left: 5px;
  }

  .character {
    font-weight: bold;
    margin: 0 5px;
  }

  .disable-character {
    color: #cccccc;
  }

  .el-switch {
    margin: 0 5px;
  }

  .cron-ico {
    cursor: pointer;
  }

</style>
