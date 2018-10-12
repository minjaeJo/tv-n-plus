<template>
  <div id="wrapper">
    <div class="status-bar">
      <a class="button is-dark center" style="width:50%;" @click="page='register/delete'">TV 프로그램 등록/삭제</a>
      <a class="button is-dark center" @click="page='schedule/rating'">편성표/순위차트</a>
    </div>
    <main>
      <div class="left-side">
        <register-program v-if="page=='register/delete'" @reload="reload"/>
        <schedule-program v-else-if="page=='schedule/rating'"/>
      </div>
      <div class="right-side">
        <delete-program v-if="page=='register/delete'" ref="delete"/>
        <rating-program v-else-if="page=='schedule/rating'" ref="rating"/>
      </div>
    </main>
  </div>
</template>

<script>
  import RegisterProgram from './LandingPage/RegisterProgram';
  import DeleteProgram from './LandingPage/DeleteProgram';
  import RatingProgram from './LandingPage/RatingProgram';
  import ScheduleProgram from './LandingPage/ScheduleProgram';
  export default {
    name: 'landing-page',
    components: {
      RegisterProgram,
      DeleteProgram,
      RatingProgram,
      ScheduleProgram
    },
    data() {
      return {
        page: 'register/delete'
      }
    },
    methods: {
      open (link) {
        this.$electron.shell.openExternal(link)
      },
      reload() {
        this.$refs.delete.loadProgram()
        this.$refs.rating.loadProgram()
      }
    }
  }
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body { font-family: 'Source Sans Pro', sans-serif; }

#wrapper {
  background:
    radial-gradient(
      ellipse at top left,
      rgba(255, 255, 255, 1) 40%,
      rgba(229, 229, 229, .9) 100%
    );
  height: 100vh;
  padding: 60px 80px;
  width: 100vw;
}
.status-bar {
  width: 100%;
  height:65px;
  text-align: center;
  margin-bottom: 25px;
}

main,
.status-bar {
  display: flex;
  justify-content: space-between;
}

main > div {
  flex-basis: 50%;
}
.button.center {
  text-align: center;
  width: 49%;
}
.left-side {
  display: flex;
  flex-direction: column;
  padding-left: 15px;
}
.right-side {
    padding-left: 30px;
}
</style>
