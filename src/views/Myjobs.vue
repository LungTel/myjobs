<template>
    <div id="myjobs">
    <el-container>
      <el-header　class="layout-left">残業申請画面</el-header>
    </el-container>

    <!--<el-radio v-model="radio" label="1">Option A</el-radio>
    <el-radio v-model="radio" label="2">Option B</el-radio>
     <el-checkbox v-model="checked">Option</el-checkbox>-->

     <p>現在の残業額・時間</p>
     <el-input class="form-input" placeholder="Please input" v-model="yen"></el-input>円
     <el-input class="form-input" placeholder="Please input" v-model="time"></el-input>時間

     <p>本日の残業時間を入力</p>
     普通: <el-input class="form-input" placeholder="Please input" v-model="inputNormalTime"></el-input>
     <br>深夜: <el-input class="form-input" placeholder="Please input" v-model="inputOverTime"></el-input>

    <el-form ref="form" :model="form" label-width="120px">
      <el-row :gutter="2">
                 <el-form-item>
                      <el-button type="primary" @click="onSubmit">確定</el-button>
                      <!-- <el-button>Cancel</el-button> -->
                 </el-form-item>
      </el-row>

    </el-form>

{{ info }}

<p v-if="errors.length">
    <b>Please correct the following error(s):</b>
    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>
</p>

    <div>
        <p>
          マイページはこちら↓
        </p>
        <router-link to="/Mypage">MyPage</router-link>
      </div>

      </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'myjobs',
   data () {
        return {
          radio: '1',
          checked: true,
          yen: '',
          time: '',
          inputNormalTime: '',
          inputOverTime: '',
          info: null,
          message: '空です',
          errors: []
        }
      },
      /*
    mounted () {
          axios
            //.get('http://localhost:8080/myjobs/hello4')
            .get('http://localhost:8080/hello',{
              params: {
                // ここにクエリパラメータを指定する
                name: 'zangyo_sisugi'
                }
              })
             .then(response => (
                this.info = response,
                this.message = response.data
                ))
              //{headers : this.headers}
                           // https://api.coindesk.com/v1/bpi/currentprice.json
                         // http://localhost:8080/myjobs/hello4
                         //.then(function(response){
                         //console.log(response);});
        },
        */
  methods: {
    onSubmit: function () {
    this.checkForm();
    this.open();
    axios.get('http://localhost:8080/workingMoney',{
                  params: {
                    yen: this.yen,
                    time: this.time,
                    inputNormalTime: this.inputNormalTime,
                    inputOverTime: this.inputOverTime
                    }
                  })
    },
    checkForm: function (e) {
          this.errors = [];

          if (!this.yen || !this.time　|| !this.inputNormalTime || !this.inputOverTime) {
            this.errors.push("フォームの値を正しく入力して下さい.");
          }

          if (!this.errors.length) {
            return true;
          }

          e.preventDefault();
        },
      open() {
                this.$message('残業申請が完了しました.');
              }
    }
}
</script>

<style>
.form-input {
    width: 60%
}
.layout-left {
  text-align: left;
}
.layout-center {
  text-align: center;
}
.layout-right {
  text-align: right;
}
.el-header, .el-footer {
    background-color: #B3C0D1;
    color: #333;
    text-align: left;
    line-height: 60px;
  }
</style>
