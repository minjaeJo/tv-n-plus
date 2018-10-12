<template>
    <div>
        <div class="title is-3 is-dark" style="margin-right: 10px;">프로그램 편성표</div>
        <div class="field">
            <div class="control">
                <input class="input is-dark" placeholder="(ex)2018-10-11" v-model="date">
            </div>
            <a class="button is-dark" @click="scheduleProgram">편성하기</a>
        </div>
        <div class="field">
            <table class="table">
                <thead>
                    <tr>
                        <th title="order">시간</th>
                        <th title="program">프로그램</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for=" program,key in schedule">
                        <th style="text-align: center;">{{schedule_time_array[key]}} ~ {{schedule_time_array[key+1]}}</th>
                        <td style="text-align: left;">{{program.title}}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>
<script>
import fs from 'fs';
import path from 'path';
export default {
    mounted() {
        this.ratingProgram()
    }
    , data() {
        return {
            date: ''
            , day_of_week: ''
            , sort_program: {
                drama: ''
                , entertainment: ''
            }
            , schedule: []
            , schedule_time : {hour: 10, minute: 0}
            , schedule_time_array: []
        }
    }
    , methods: {
        getDayOfTheWeek() {
            let day_of_the_week = new Date(this.date).getDay();
            if(day_of_the_week == '0' || day_of_the_week == '6') {
                return 'weekend';
            } else {
                return 'weekday';
            }
        }
        , ratingProgram() {
            const json_container = fs.readFileSync(path.join(__static, '/data.json'), 'utf8');
            let array = JSON.parse(json_container);
            let drama_array = array.program.filter(data => data.genre == '드라마');
            let entertainment_array = array.program.filter(data => data.genre == '예능');
            this.sort_program.drama = this.sortingArray(drama_array);
            this.sort_program.entertainment = this.sortingArray(entertainment_array);
        }
        , sortingArray(array) {
            return array.sort((prev,next) => {
                return next['rating'] - prev['rating'];
            })
        }
        , calcTime(prev_time, runtime) {
            let next_time = prev_time;
            if(Number(next_time.minute) + Number(runtime) >= 60) {
                next_time.hour = Number(next_time.hour) + 1;
                next_time.minute = Number(next_time.minute) + Number(runtime) - 60;
                if(Number(next_time.minute)>=60){
                    next_time.hour = Number(next_time.hour) + 1;
                    next_time.minute = Number(next_time.minute) - 60;
                }
            } else {
                if(Number(next_time.minute)>=60){
                    next_time.hour = Number(next_time.hour) + 1;
                    next_time.minute = Number(next_time.minute) - 60;
                } else {
                    next_time.minute = Number(next_time.minute) + Number(runtime);
                }
            }
            if(Number(next_time.minute) < 10) {
               next_time.minute = '0'+ String(next_time.minute)
            }
            return next_time;
        }
        , scheduleProgram() {
            this.schedule = '';
            this.day_of_week = '';
            this.schedule_time = {hour: 10, minute: 0};
            this.schedule_time_array = [];
            console.log(this.getDayOfTheWeek())
            let drama = this.sort_program.drama;
            let entertainment = this.sort_program.entertainment;
            switch (this.getDayOfTheWeek()) {
                case 'weekday':
                    this.schedule = [
                        drama[3],
                        drama[2],
                        entertainment[1],
                        entertainment[3],
                        drama[1],
                        drama[0],
                        entertainment[0],
                        entertainment[2]
                    ];
                    this.day_of_week = '평일';
                    break;
                case 'weekend':
                    this.schedule = [
                        drama[3],
                        drama[2],
                        entertainment[1],
                        entertainment[0],
                        entertainment[2],
                        entertainment[3],
                        drama[0],
                        drama[1]
                    ];
                    this.day_of_week = '주말';
                    break;
            }
            this.schedule.forEach((element)=> {
                let time= this.calcTime(this.schedule_time, element.runtime);
                this.schedule_time_array.push(String(time.hour)+':'+String(time.minute))
                console.log(element);
                // console.log(String(prev_time.hour)+':'+String(prev_time.minute)+ ' ~ ' + String(time.next_time.hour)+':'+String(time.next_time.minute))
            })
        }
    }
}
</script>
<style scoped>
.control {
    width: 50%;
    display: inline-block;
    vertical-align: middle;
    margin: 0 10px;
}
table {
    margin-top: 10px;
    background: none;
}
</style>