<template>
  <div class="index-index">
    <div style="padding-right: 12px;padding-left: 6px;">
      <el-row :gutter="20">
        <el-col :span="24">
          <el-table :data="mobilesuitupdatalist.list" style="width: 100%;text-align: center;">
            <!--<el-table-column label="序号">
              <template slot-scope="scope">
                <p>
                  {{scope.$index}}
                </p>
              </template>
            </el-table-column>-->
            <el-table-column prop="ID" label="ID">
            </el-table-column>
            <el-table-column label="操作" fixed="right">
              <template slot-scope="scope">
                <p>
                  <span class="font-green" @click="restnow(scope.row)">修改</span>
                  <span class="font-green" @click="delect(scope.$index)">删除</span>
                </p>
              </template>
            </el-table-column>
          </el-table>
        </el-col>
      </el-row>
    </div>
    <el-dialog :title="titleName" :visible.sync="mobilesuitShow" :fullscreen="true">
      <el-form :inline="true" class="demo-form-inline">
        <el-form-item label="当前机体">
          <div style="width: 160px;">
            <img :src="msinform.img" />
            <el-select v-model="selectms" placeholder="请选择" @change="nowms">
              <el-option label="空" value="{}"></el-option>
              <el-option v-for="item in gundamlist" :key="item.id" :label="item.name" :value="item.code">
              </el-option>
            </el-select>
            <el-input v-model="mobilesuitupdata.ID" placeholder="请输入id" @change="restnow(mobilesuitupdata)" clearable></el-input>
          </div>
        </el-form-item>
        <el-form-item label="Skill">
          <el-input v-model="mobilesuitupdata.Skill" clearable></el-input>
        </el-form-item>
        <el-form-item label="Lv">
          <el-input v-model="mobilesuitupdata.Lv" clearable></el-input>
        </el-form-item>
        <el-form-item label="Exp">
          <el-input v-model="mobilesuitupdata.Exp" clearable></el-input>
        </el-form-item>
        <el-form-item label="場數">
          <el-input v-model="mobilesuitupdata.Sess" clearable></el-input>
        </el-form-item>
        <el-form-item label="Charge">
          <el-input v-model="mobilesuitupdata.Charge" clearable></el-input>
        </el-form-item>

        <el-form-item label="涂装1">
          <el-color-picker v-model="mobilesuitupdata.Color[0]" clearable></el-color-picker>
        </el-form-item>
        <el-form-item label="涂装2">
          <el-color-picker v-model="mobilesuitupdata.Color[1]" clearable></el-color-picker>
        </el-form-item>
        <el-form-item label="涂装3">
          <el-color-picker v-model="mobilesuitupdata.Color[2]" clearable></el-color-picker>
        </el-form-item>
        <el-form-item label="涂装4">
          <el-color-picker v-model="mobilesuitupdata.Color[3]" clearable></el-color-picker>
        </el-form-item>
        <el-form-item label="涂装5">
          <el-color-picker v-model="mobilesuitupdata.Color[4]" clearable></el-color-picker>
        </el-form-item>
        <el-form-item label="涂装6">
          <el-color-picker v-model="mobilesuitupdata.Color[5]" clearable></el-color-picker>
        </el-form-item>

        <el-form-item label="拋光">
          <el-input v-model="mobilesuitupdata.Polish" clearable></el-input>
        </el-form-item>

        <el-form-item label="纹章1">
          <el-input v-model="mobilesuitupdata.Coat[0]" clearable></el-input>
        </el-form-item>
        <el-form-item label="纹章2">
          <el-input v-model="mobilesuitupdata.Coat[1]" clearable></el-input>
        </el-form-item>
        <el-form-item label="纹章3">
          <el-input v-model="mobilesuitupdata.Coat[2]" clearable></el-input>
        </el-form-item>
      </el-form>
      <el-row>
        <el-col :span="22" align="end" :offset="1">
          <el-button type="primary" size="small" @click="mobileupdata">修改</el-button>
          <el-button size="small" @click="mobilesuitShow=false">取消</el-button>
        </el-col>
      </el-row>
    </el-dialog>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        msinform: {
          color1: '',
          color2: '',
          color3: '',
          color4: '',
          color5: '',
          color6: '',
        },
        mySwiper: "",
        value: {},
        selectms: "{}",
        mySwipertwo: "",
        userid: {},
        gundamlist: [],
        mobilesuitupdatalist: {},
        login: {},
        titleName: "修改",
        openShow: false,
        mobilesuitShow: false,
        mobilesuitupdata: {
          Color: [0, 0, 0, 0, 0, 0],
          Coat: [0, 0, 0],
        },
        searchdata: {
        },
        adddata: {},
        updatadata: {
        }
      }
    },
    mounted() {
      this.getgundamlist();
      this.getmobilesuitupdatalist();
    },
    methods: {
      handleChange(val) {
      },
      register() {},
      nowms() {
        console.log(11)
        if(this.selectms == "{}") {
          this.msinform.code = "0";
          this.msinform.img = "0";
        } else {
          this.mobilesuitupdata.ID = this.selectms.toString();
          for(let i = 0; i < this.gundamlist.length; i++) {
            if(this.gundamlist[i].code == this.selectms) {
              this.msinform.img = this.gundamlist[i].img;
              this.selectms = this.gundamlist[i].code.toString()
              this.mobilesuitupdata.ID = this.gundamlist[i].code;
            }
          }
        }
      },
      restnow(item) {
        this.mobilesuitShow = true;
        this.mobilesuitupdata = item;
        this.mobilesuitupdata.Color = this.json2color(item.Color)
        this.selectms = "{}";
        if(item) {
          for(let i = 0; i < this.gundamlist.length; i++) {
            if(this.gundamlist[i].code == this.mobilesuitupdata.ID) {
              this.msinform.img = this.gundamlist[i].img;
              this.selectms = this.gundamlist[i].code.toString()
              this.mobilesuitupdata.ID = this.gundamlist[i].code;
            }
          }
        };
      },
      getmobilesuitupdatalist() {
          this.userid = sessionStorage.nameid;
          this.$http({
            url: this.$http.adornUrl("api/bot"),
            method: 'GET',
            params: this.$http.adornParams(this.searchdata)
          }).then(({
            data
          }) => {
            console.log(data)
            this.mobilesuitupdatalist = data
          });
      },
      getgundamlist() {
        this.$http({
          url: 'http://39.105.85.21:3001/gundamlist',
          method: 'GET',
          params: this.$http.adornParams(this.searchdata)
        }).then(({
          data
        }) => {
          this.gundamlist = data.list
        });
      },
      json2color(colorlist) {
        var out = [];
        for(var i = 0; i < colorlist.length; i++) {
          out.push('#' + colorlist[i]);
        }
        return out
      },
      color2json(colorlist) {
        var out = [];
        for(var i = 0; i < colorlist.length; i++) {
          if(colorlist[i] == null) {
            out.push('0');
          } else {
            out.push(colorlist[i].split("#")[1]);
          }
        }
        return out
      },
      colortrans2(colorcode) {
        var mycolors = "";
        if(colorcode) {
          mycolors = colorcode.split("#")[1];
        }
        return mycolors;
      },
      mobileupdata() {
        this.mobilesuitupdata.Color = this.color2json(this.mobilesuitupdata.Color);
        this.$http({
          url: this.$http.adornUrl("api/bot"),
          method: 'POST',
          data: this.$http.adornData(this.mobilesuitupdatalist)
        }).then(({
          data
        }) => {
          this.getmobilesuitupdatalist();
        }).catch(({
          data
        }) => {
        });
      },
      delect(index) {
        this.mobilesuitupdatalist.list.splice(index, 1)
        console.log(this.mobilesuitupdatalist.list)
        this.$http({
          url: this.$http.adornUrl("api/bot"),
          method: 'POST',
          data: this.$http.adornData(this.mobilesuitupdatalist)
        }).then(({
          data
        }) => {
          this.getmobilesuitupdatalist();
        }).catch(({
          data
        }) => {
        });
      }
    }
  }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
