<template>
  <el-row class="menu_title_wrap home">
    <el-col :span="1" style="height:10px"></el-col>
    <el-col :span="22">
      <div class="li4_parts">
        <div class="alertTip alertYDMD" style="width: 1452px;height: 786px;" v-show="hisBand">
          <div class="alertBody " style="margin: -330px -550px;width: 1100px;height: 660px;">
            <div class="bodyHead"><div class="title">绑卡记录</div>
              <select  style="position: absolute;margin: 9px -411px;font-size: 19px;background: none;border: none;" name="" id="">
                <option v-on:click="selectDate(0)" selected="selected">1天内记录</option>
                <option v-on:click="selectDate(1)">2天内记录</option>
                <option v-on:click="selectDate(2)">3天内记录</option>
                <option v-on:click="selectDate(3)">4天内记录</option>
                <option v-on:click="selectDate(4)">5天内记录</option>
                <option v-on:click="selectDate(5)">6天内记录</option>
                <option v-on:click="selectDate(6)">7天内记录</option>
              </select>
              <div  class="close" v-on:click="hisBand=false">X</div>
            </div>
            <div class="bodyCon" style="height: 514px;" >
              <table  border="2" cellspacing="0" width="100%">
                <tr>
                  <th>关联罪犯	</th>
                  <th>点位号	</th>
                  <th>IC卡号	</th>
                  <th>操作民警	</th>
                  <th>操作时间	</th>
                  <th>解绑类型	</th>
                  <th>操作地点	</th>
                </tr>
                <tr v-for="his in hisList" :key="1">
                  <td>{{his.CriminalName}}</td>
                  <td>{{his.PSID}}</td>
                  <td>{{his.ICCard}}</td>
                  <td>{{his.OperationPoliceName}}</td>
                  <td>{{(his.ChangeTime==""||his.ChangeTime==null)?"":his.ChangeTime.replace("T"," ")}}</td>
                  <td>{{his.BindTypeName}}</td>
                  <td>{{his.CardAreaName}}</td>
                </tr>
              </table>
            </div>
            <!--<el-row >-->
            <!--<el-col :span="8" style="height: 10px"></el-col>-->
            <!--<el-col :span="8" >-->
            <!--<div class="pages">-->
            <!--<span class="pageControl"><img src="../../assets/q1.png" v-on:click="getCriminalback()" alt=""/></span>-->
            <!--<span class="pagesText">{{criminalPage+1}}/{{Math.ceil(criminalCount/18)==0?1:Math.ceil(criminalCount/18)}}</span>-->
            <!--<span class="pageControl"><img src="../../assets/q2.png" v-on:click="getCriminalGo()" alt=""/></span>-->
            <!--</div>-->
            <!--</el-col>-->
            <!--<el-col :span="8" style="height: 10px"></el-col>-->
            <!--</el-row>-->
          </div>
        </div>

        <!--<div class="tabHead">-->
          <!--<div  :class="['tab', { tabing: isB1}]"  @click="bandCardInfo_onBind()">绑定</div>-->
          <!--<div  :class="['tab', { tabing: isB2}]"  @click="UnbandCardInfo_onUnBind()">解绑</div>-->
        <!--</div>-->
        <div class="tab1">
          <div class="partsBody">
            <div class="bodyHead">
              <div class="title">人员解绑</div>
            </div>
            <div class="bodyCon">
              <el-row >
                  <el-row class="float_person_wrap">
                    <el-col :span="4" v-for="(item,index) in chest_card" :key='1' v-show="!isUnbind">
                      <div class="float_person_card card_bind_init" :class="['card_bind_init', {card_bind_select: item.status}, {card_bind_success:item.wristband!==''}]" v-on:contextmenu.prevent ="$emit('delCardSelect',index)" @click="$emit('bindCardSelect',index)">
                        <el-col :span="10" class="photo">
                          <img :src="item.Photo" alt="" width="100%" height="100%">
                        </el-col>
                        <el-col :span="12" class="crimal_content">
                          <p>姓名：{{item.CriminalName}}</p>
                          <!--<p>罪犯编号：{{item.CriminalID}}</p>-->
                          <!--<p>番号：{{item.CriminalNum}}</p>-->
                          <p>胸牌编号：{{item.RFID}}</p>
                          <p>腕带编号：{{item.wristband}}</p>
                        </el-col>
                      </div>
                    </el-col>
                    <el-col :span="4" v-for="item in wristband" :key='1' v-show="isUnbind">
                      <div class="float_person_card card_bind_success">
                        <el-col :span="10" class="photo">
                          <img :src="item.Photo" alt="" width="100%" height="100%">
                        </el-col>
                        <el-col :span="12" class="crimal_content">
                          <p>姓名：{{item.CrimalName}}</p>
                          <!--<p>罪犯编号：{{item.CriminalID}}</p>-->
                          <p>腕带编号：{{item.RFID}}</p>
                        </el-col>
                      </div>
                    </el-col>
                  </el-row>
              </el-row>

            </div>

          </div>
          <div class="partsFoot">
            <div class="alertText">{{alertTip}}</div>

            <div style="margin: 11px 2px;float: right">
              <!--<input class="sureAble" value="提交"  type="button" @click="bandCardInfoSubmit()"  >-->
              <input class="sureAble" value="提交解绑" type="button" @click="bandCardInfoUnbind()" >
              <input class="sureAble"  value="一键解绑" type="button" @click="bandCardUnbindAll()" >
              <input class="sureAble"  value="取消" type="button" @click="BindCancel()">
              <input class="sureAble" value="解卡记录"  type="button" @click="hisShow()">

            </div>
          </div>
        </div>

      </div>
    </el-col>
    <el-col :span="1"  style="height:10px"></el-col>
  </el-row>

</template>

<script>
  import { BasicUrl,IMG,ReceiveAjaxUrl,MapUrl,ajaxUrl } from '../../config'

  export default {
    name: 'navheader',
    props:[
      'chest_card',
      'wristband'
    ],
    data () {
      return {
        isUnbind:false,
        CardTitle:'卡绑定',
        isB1: false,
        isB2: true,
        alertTip: "",
        hisBand:false,
        hisList:[]
      }
    },
    methods: {
      /*卡绑定取消*/
      BindCancel:function () {
        var vm=this
        var send3 = {
          Header: {
            MsgID:"201501260000000035",
            MsgType:26
          },
          Body: JSON.stringify({
            OrgID : localStorage.getItem('OrgID'),
            DoorID : localStorage.getItem('DoorID')
          })
        }
        //发送数据
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(send3),
          success: function (result) {
            localStorage.setItem("moveTypes","0")
            vm.$router.push({ path: '/' })
          },
          complete: function (XHR, TS) {
            XHR = null;  //回收资源
          }
        })

      },
      /* 绑定 */
      bandCardInfo_onBind:function () {
        let vm = this
        this.isB1 =  true
        this.isB2 = false
        vm.$emit('CardBindPageInit')
        vm.CardTitle = '卡绑定'
        vm.isUnbind = false
        var bandCardInfo_req = {
          Header: {
            MsgID:"201501260000000001",
            MsgType:50,
          },
          Body: JSON.stringify({
            DoorID : vm.getLocalStorage('DoorID'),
            RegType:4601
          })
        }
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(bandCardInfo_req),
          success: function (result) {
            console.log('开始绑定',result)
          },
          complete: function (XHR) {
            XHR = null;  //回收资源
          }
        });
      },

      /* 提交绑定 */
      bandCardInfoSubmit:function () {
        let vm = this
        vm.addDisable()
        let ChangeCardPeopleList = []
        for(let i = 0; i<vm.chest_card.length; i++){
          if(vm.chest_card[i].wristband == null || vm.chest_card[i].wristband == ''){
//            alert('请绑定腕带')
            vm.alertTip="请绑定腕带"
            setTimeout(function () {
              vm.alertTip=""
            },2000)
            return
          }
        }
        for(let i = 0; i<vm.chest_card.length; i++){
          ChangeCardPeopleList.push({
            CriminalID:vm.chest_card[i].CriminalID,
            ChestCard:vm.chest_card[i].CardID,
            WristCard:vm.chest_card[i].wristband
          })
        }
        var bandCardInfoSubmit = {
          Header: {
            MsgID:"201501260000000001",
            MsgType:52,
          },
          Body: JSON.stringify({
            DoorID : vm.getLocalStorage('DoorID'),
            ChangeCardPeopleList:ChangeCardPeopleList
          })
        }
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(bandCardInfoSubmit),
          success: function (result) {
            vm.delDisable()
            if(result.RET === 1){
//              alert('绑定成功')
              localStorage.setItem("moveTypes","0")
              vm.alertTip="绑定成功"
              setTimeout(function () {
                vm.alertTip=""
                vm.$router.push({ path: '/' })
              },1000)
            } else {
              /*alert('绑定失败')*/
              vm.alertTip="绑定失败"
              setTimeout(function () {
                vm.alertTip=""
              },2000)
            }
          },
          complete: function (XHR) {
            XHR = null;  //回收资源
          },
          error:function () {
            vm.delDisable()
          }
        });
      },
      /*阻止点击提交*/
      addDisable:function () {
        $('.sureAble').attr("disabled","disable");
//        $('.sureAble').removeAttr("disabled");
//        $(".title").html($('.sureAble'))
      },
      /*解除点击提交*/
      delDisable:function () {
        $('.sureAble').removeAttr("disabled");
      },
      /* 解绑 */
      UnbandCardInfo_onUnBind:function () {
        let vm = this
        this.isB1 = false
        this.isB2 = true
        vm.$emit('clearCardInfo')
        vm.CardTitle = '卡解绑'
        vm.isUnbind = true
        var bandCardInfo_req = {
          Header: {
            MsgID:"201501260000000001",
            MsgType:50,
          },
          Body: JSON.stringify({
            DoorID : vm.getLocalStorage('DoorID'),
            PoliceID : localStorage.getItem('BindplacemanID'),
            RegType:4603
          })
        }
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(bandCardInfo_req),
          success: function (result) {
            console.log('开始解绑',result)
          },
          complete: function (XHR) {
            XHR = null;  //回收资源
          }
        });
      },
      /* 提交解绑 */
      bandCardInfoUnbind:function () {
        let vm = this
        if(vm.wristband.length==0){
          vm.alertTip="无卡解绑"
          setTimeout(function () {
            vm.alertTip=""
          },2000)
        }else {
          vm.addDisable()
          let UnBundingList = []
          for(let i = 0; i<vm.wristband.length; i++){
            UnBundingList.push({
              CriminalID:vm.wristband[i].CriminalID,
              WristCard:vm.wristband[i].CardID,
            })
          }
          var UnbandCardInfoSubmit = {
            Header: {
              MsgID:"201501260000000001",
              MsgType:53,
            },
            Body: JSON.stringify({
              DoorID : vm.getLocalStorage('DoorID'),
              PoliceID : localStorage.getItem('BindplacemanID'),
              UnBundingList:UnBundingList
            })
          }
          $.ajax({
            type: "get",
            contentType: "application/json; charset=utf-8",
            dataType: "jsonp",
            jsonp: "callback",
            async: false,
            url: ajaxUrl,
            data:JSON.stringify(UnbandCardInfoSubmit),
            success: function (result) {
              vm.delDisable()

              if(result.RET === 1){
//              alert('解除绑定成功')
                vm.alertTip="解除绑定成功"
                localStorage.setItem("moveTypes","0")

                setTimeout(function () {
                  vm.alertTip=""
                  vm.$router.push({ path: '/' })
                },1000)

              } else {
//              alert('解除绑定失败')
                vm.alertTip="解除绑定失败"
                setTimeout(function () {
                  vm.alertTip=""
                },2000)
//              vm.$router.push({ path: '/' })
              }
            },
            complete: function (XHR) {
              XHR = null;  //回收资源
            },
            error:function () {
              vm.delDisable()
            }
          });
        }

      },
      /* 一键解绑 */
      bandCardUnbindAll:function () {
        let vm = this
        vm.addDisable()
        var bandCardInfo_reqAll = {
          Header: {
            MsgID:"201501260000000001",
            MsgType:50,
          },
          Body: JSON.stringify({
            DoorID : vm.getLocalStorage('DoorID'),
            Police: localStorage.getItem('BindplacemanID'),
            RegType:4602
          })
        }
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(bandCardInfo_reqAll),
          success: function (result) {
            vm.delDisable()
            if(result.RET === 1){
//              alert('一键解绑成功')
              vm.alertTip="一键解绑成功"
              setTimeout(function () {
                vm.alertTip=""
                vm.$router.push({ path: '/' })
              },1000)
              localStorage.setItem("moveTypes","0")


            } else {
//              alert('一键解绑失败')
              vm.alertTip="一键解绑失败"
              setTimeout(function () {
                vm.alertTip=""
              },2000)
            }
          },
          complete: function (XHR) {
            XHR = null;  //回收资源
          },
          error:function () {
            vm.delDisable()
          }
        });
      },
      /*历史记录*/
      hisShow:function () {
        var vm=this
        this.hisBand=true
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          data:{
            AreaID:localStorage.getItem("AreaID"),
            Type:"4603,4602",
            Duration:0
          },
          url: BasicUrl + 'ChangeCard/GetChangeCardRecords',
          success: function (result) {
            console.log(result)
            vm.hisList=result

          }
        })


      },
      selectDate:function (X) {
        var vm=this
        this.hisBand=true
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          data:{
            AreaID:localStorage.getItem("AreaID"),
            Type:4603,
            Duration:X
          },
          url: BasicUrl + 'ChangeCard/GetChangeCardRecords',
          success: function (result) {
            console.log(result)
            vm.hisList=result

          }
        })
      }
    },
    mounted () {
      var vm = this
      /* Coding By YanM */
      localStorage.setItem("placemanID","0")
      vm.$emit('CardBindPageInit')
      var outPlice= setInterval(function () {
        if(localStorage.getItem("placemanID")==0){
           /*民警还未刷卡*/
        }else if(localStorage.getItem("placemanID")==1){
           /* 点击登录框关闭按钮停止检测民警登录情况*/
          clearInterval(outPlice)
        }else{
          localStorage.setItem("moveTypes","4")//1为进出工，2为临时外出登记，3为卡绑定,4为卡解绑
          vm.UnbandCardInfo_onUnBind()
          vm.$emit('CardBindPageInit')
          clearInterval(outPlice)
        }
      },1000)
      $(".sureAble").focus(function(){this.blur()});


      /* Coding By YanM */
    }

  }
</script>

<style lang="scss" scoped>
  input,textarea:focus {
    outline: none;
  }
  .float_person_card{
    padding: 10px;
    height: 150px;
    margin: 10px 10px;
    .photo{
      height:100%;
    }
  }
  .outperson{
    background: #196fff;
  }
  .illegal{
    background: #93374e;
  }
  .crimal_content{
    color: #fff;
    font-size: 12px;
    text-align: left;
    margin-left: 10px;
    overflow: hidden;
    p{
      margin: 0;
      line-height: 22px;
      /*white-space: nowrap;*/
      /*text-overflow: ellipsis;*/
    }
  }
  .card_bind_select{
    box-shadow: inset 0px 0px 9px 3px #000000;
  }
  .card_bind_success{
    background: #196efc !important;
    /*box-shadow: none ;*/
  }
  .card_bind_init{
    background: #109b62;
  }






  .li4_parts {
    height: 740px;
    margin: 0px auto;
  }

  .li4_parts .partsBody{
    width: 100%;
    height: 695px;
    background: white;

  }
  .li4_parts .partsBody {
    width: 100%;
    height: 702px;
    background: white;
  }
  .li4_parts .partsFoot{
    width: 100%;
    height: 84px;
    /*margin-top: -48px;*/
    background: #c5cfdb;

  }
  .li4_parts .bodyCon{
    height: 592px;
    padding: 20px;
    overflow: auto;
  }
  .li4_parts .sureAble{
    width: 126px;
    height: 40px;
    background: #004bdc;
    text-align: center;
    color: white;
    font-size: 20px;
    line-height: 36px;
    letter-spacing: 2px;
    float: right;
    margin: 0px 35px;
  }

  .li4_parts .bodyHead{
    width: 100%;
    height: 46px;
    background: #c5cfdb;
  }
  .li4_parts .title{
    font-size: 25px;
    font-weight: 800;
    color: #1d68e8;
    text-shadow: 2px 2px 2px #fff;
    line-height: 45px;
    text-indent: 15px;
    float: left;

  }
  .li4_parts .tabHead{
    width: 100%;
    height: 40px;
  }
  .li4_parts .tab{
    width: 155px;
    height: 40px;
    background: #004bdc;
    font-size: 18px;
    text-align: center;
    float: left;
    color:white;
    line-height: 38px;
  }
  .li4_parts .tabing{
    background: white;
    font-size: 18px;
    color: #004bdc;
    text-align: center;
  }


  tr:nth-child(2n){background:#cac9c6;}
</style>
