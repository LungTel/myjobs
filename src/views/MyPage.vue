<template>
    <div id="mypage">
    <el-row>
      <el-col :span="24"><div class="grid-content bg-purple-dark">マイページ</div></el-col>
    </el-row>

     名前: <el-input placeholder="Please input" v-model="name"></el-input>
     ID: <el-input placeholder="Please input" v-model="id"></el-input>
     e-mail: <el-input placeholder="Please input" v-model="mailaddress"></el-input>
     時間単価(普通): <el-input placeholder="Please input" v-model="lowrate"></el-input>
     時間単価(深夜): <el-input placeholder="Please input" v-model="highrate"></el-input>
     締日: <el-input placeholder="Please input" v-model="enddate"></el-input>

    <el-form ref="form" :model="form" label-width="120px">
      <el-row :gutter="2">
                 <el-form-item>
                      <el-button type="primary" @click="onSubmit">更新</el-button>
                      <!-- <el-button>Cancel</el-button> -->
                 </el-form-item>
      </el-row>
    </el-form>

<p v-if="errors.length">
    <b>Please correct the following error(s):</b>
    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>
</p>

    <div>
            <p>
              ホームに戻る↓
            </p>
            <router-link to="/Myjobs">HOME</router-link>
    </div>

</div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'mypage',
   data () {
        return {
          name: '',
          id: '',
          mailaddress: '',
          lowrate: '',
          highrate: '',
          enddate: '',
          message: '空です',
          errors: []
        };
      },
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

                if (!this.name || !this.id　|| !this.mailaddress || !this.lowrate || !this.highrate || !this.enddate) {
                  this.errors.push("フォームの値を正しく入力して下さい.");
                }

                if (!this.errors.length) {
                  return true;
                }

                e.preventDefault();
          },
          open() {
                this.$message('登録が完了しました.');
          }
       }

}

</script>

<style>
  .el-row {
    margin-bottom: 20px;
    &:last-child {
      margin-bottom: 0;
    }
  }
  .el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #409EFF;
  }
  .bg-purple {
    background: #d3dce6;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }
</style>
