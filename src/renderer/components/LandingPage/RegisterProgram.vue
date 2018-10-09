 npm-windows-upgrade
<template>
    <div>
        <div>
            <div class="title is-3 is-dark" style="margin-right: 10px;">프로그램 등록</div>
            <a class="button is-dark" @click="saveFSData">추가하기</a>
        </div>
        <div class="field">
            <span class="title is-6 is-dark" style="margin-right: 16px;">제목:</span>
            <div class="control">
                <input class="input is-dark" v-model="tv_program.title" placeholder="(ex)신사유기">
            </div>
        </div>
        <div class="field">
            <span class="title is-6 is-dark" style="margin-right: 16px;">장르:</span>
            <div class="control">
                <input class="input is-dark" v-model="tv_program.genre" placeholder="(ex)드라마/예능">
            </div>
        </div>
        <div class="field">
            <span class="title is-6 is-dark">시청률:</span>
            <div class="control">
                <input class="input is-dark" v-model="tv_program.rating" placeholder="(ex)7.2%">
            </div>
        </div>
        <div class="field">
            <span class="title is-6 is-dark">런타임:</span>
            <div class="control">
                <input class="input is-dark" v-model="tv_program.runtime" placeholder="(ex)80min">
            </div>
        </div>
        
    </div>
</template>

<script>
import fs from 'fs';
import path,{ normalize } from 'path';
export default {
    mounted() {
    },
    data () {
        return {
            tv_program: {
                title: '',
                genre: '',
                rating: '',
                runtime: ''
            }
        }
    },
    methods: {
        saveFSData() {
            /*
            *   json_container: 파일시스템에서 data.json 읽은 정보
            *   base_format: data.json에 공백일 경우 생성되는 기초 포맷 정보값
            *   program_array: JSON 파일 형식을 Parse한 값
            *   writeFileSync: 파일을 저장 가능하도록 도와주는 파일 시스템 메소드
            *   readFileSync: 파일을 읽도록 도와주는 파일시스템 메소드
            */ 
            const json_container = fs.readFileSync(path.join(__static, '/data.json'), 'utf8');
            if(json_container === '') {
                let base_format = {program:[]}
                fs.writeFileSync(path.join(__static, '/data.json'), JSON.stringify(base_format),'utf8');
                this.initData();
                alert('data.json의 기초 포맷을 생성합니다. 다시 입력하세요.')
            }
            let program_array = JSON.parse(json_container);
            program_array.program.push(this.tv_program);
            fs.writeFileSync(path.join(__static, '/data.json'), JSON.stringify(program_array),'utf8');
            alert(this.tv_program.title + '해당 방송 프로그램 정보를 추가합니다.')
            this.initData();
            this.reloadOther();
        },
        reloadOther() {
            this.$emit('reload');
        },
        // 데이터 초기화 메소드
        initData() {
            this.tv_program.title = '';
            this.tv_program.genre = '';
            this.tv_program.rating = '';
            this.tv_program.runtime = '';
        },
        // 정규표현식 함수 제작 필요
        normalizeData() {

        }
    }
}
</script>

<style scoped>
.field {
    margin-left: 15px;
}
.control {
    width: 50%;
    display: inline-block;
    vertical-align: middle;
    margin-left: 10px;
}
.title {
    display: inline-block;
}
</style>
