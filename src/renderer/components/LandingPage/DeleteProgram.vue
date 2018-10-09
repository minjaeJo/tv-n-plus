 npm-windows-upgrade
<template>
  <div>
    <div>
      <div class="title is-3 is-dark" style="margin-right: 10px;">프로그램 삭제</div>
      <a class="button is-dark" @click="deleteProgram">삭제하기</a>
    </div>
    <div class="control">
      <div class="select is-dark">
        <select v-model="delete_value">
          <option disabled value=''>삭제할 프로그램을 골라주세요</option>
          <option v-for="program in program_array.program" :value="program.title">{{program.title}}</option>
        </select>
      </div>
      
    </div>
  </div>
</template>

<script>
import fs from 'fs';
import path from 'path';
  export default {
    created() {
      this.loadProgram(); 
    },
    data () {
      return {
        program_array: '',
        delete_value: ''
      }
    },
    methods: {
      loadProgram() {
        const json_container = fs.readFileSync(path.join(__static, '/data.json'), 'utf8');
        this.program_array = JSON.parse(json_container);
      },
      deleteProgram() {
        this.program_array.program.forEach(element => {
          this.program_array.program.splice(element.title.indexOf(this.delete_value),1);
        });
        fs.writeFileSync(path.join(__static, '/data.json'), JSON.stringify(this.program_array),'utf8');
        this.loadProgram();
      },
      allDeleteProgram() {

      }
    }
  }
</script>

<style scoped>
.title {
  display: inline-block;
}
.control {
  margin-left: 10px;
}
</style>
