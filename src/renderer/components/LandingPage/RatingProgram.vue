<template>
    <div>
        <div>
            <div class="title is-3 is-dark" style="margin-right: 10px;">프로그램 순위표</div>
        </div>
        <div class="field">
            <table class="table">
                <thead>
                    <tr>
                        <th title="order">순위</th>
                        <th title="program">프로그램</th>
                        <th title="program">장르</th>
                        <th title="program">시청률</th>
                        <th title="program">런타임</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for=" program, key in program_array">
                        <th style="text-align: center;">{{key + 1}}</th>
                        <td style="text-align: left;">{{program.title}}</td>
                        <td style="text-align: left;">{{program.genre}}</td>
                        <td style="text-align: center;">{{program.rating}}</td>
                        <td style="text-align: center;">{{program.runtime}}</td>
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
    },
    data() {
        return {
            program_array: ''
        }
    },
    methods: {
        ratingProgram() {
            const json_container = fs.readFileSync(path.join(__static, '/data.json'), 'utf8');
            let array = JSON.parse(json_container);
            this.program_array = array.program.sort((prev,next)=> {
                return next['rating'] - prev['rating'];
            })
        }
    }
}
</script>
<style scoped>
table {
    margin-top: 10px;
    background: none;
}
</style>


