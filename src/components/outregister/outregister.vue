<template>
  <!--<div class="navheader">人员清点</div>-->
  <el-row class="menu_title_wrap home">
    <el-col :span="1" style="height:10px"></el-col>
    <el-col :span="22">
      <div class="li3_parts">
        <!--键盘模块Star-->
        <div  class="searchMode" v-show="searchModeShow">
          <p style="font-size: 20px;color: #f00000;">请输入外出时长（分钟）</p>
          <input class="searchInput" maxlength="18" v-model="seachNum"  type="text">
          <div class="searchBottons">
            <div class="listY"  v-on:click="enterNum('1')">1</div>
            <div class="listY" v-on:click="enterNum('2')">2</div>
            <div class="listY" v-on:click="enterNum('3')">3</div>
            <div class="listY" v-on:click="enterNum('4')">4</div>
            <div class="listY" v-on:click="enterNum('5')">5</div>
            <div class="listY" v-on:click="enterNum('6')">6</div>
            <div class="listY" v-on:click="enterNum('7')">7</div>
            <div class="listY" v-on:click="enterNum('8')">8</div>
            <div class="listY" v-on:click="enterNum('9')">9</div>
            <div class="listY" v-on:click="enterNum('0')">0</div>
            <div class="listY" v-on:click="enterNum('-')"style="font-size: 23px;width:77px;">清除</div>

            <div class="listY" style="font-size: 23px;" v-on:click="setTime()">确定</div>
            <!--<div class="listY" style="font-size: 23px;" v-on:click="searchModeShow=false">关闭</div>-->
          </div>
        </div>
        <!--键盘模块End-->
        <!--<div class="tabHead"style="height:40px;">-->
          <!--<div  :class="['tab', { tabing: isB1}]"  v-on:click="toggle1()">外出登记</div>-->
          <!--<div  :class="['tab', { tabing: isB2}]"  v-on:click="toggle2()">登记记录</div>-->
        <!--</div>-->
        <div class="tab1" v-show="isShow1">
          <div class="partsBody">
            <div class="bodyHead">
              <div class="title">外出登记</div>
            </div>
            <div class="bodyCon">
              <el-row >
                <el-col :span="12">

                  <el-row >
                    <el-row >
                      <div class="deailHead">
                        外出事由 <span   v-show="TimeShow" style="font-size: 16px;color: #f53a2a;font-weight: 800;">(外出时长{{seachNum}}分钟)</span>
                      </div>
                      <div class="deailBody" style="height:269px;">
                        <el-col :span="4" v-for="(reason,index) in reasonList.slice(reasonA-1,reasonB)" :key="1">
                          <div :class="['choose', {choosed: reason.ischoose}]" v-on:click="chooseReason(index,reason.Str3,reason.Str4)"  >
                            {{reason.DictCodeName}}
                          </div>
                        </el-col>
                      </div>
                    </el-row>
                    <el-row >
                      <el-col :span="3" style="height: 10px"></el-col>
                      <el-col :span="18" >
                        <div class="pages">
                          <span class="pageControl"><img src="../../assets/q1.png" v-on:click="ReasonBack()" alt=""/></span>
                          <span class="pagesText">{{reasonNowPage}}/{{reasonPages}}</span>
                          <span class="pageControl"><img src="../../assets/q2.png" v-on:click="ReasonGo()" alt=""/></span>
                        </div>
                      </el-col>
                      <el-col :span="3" style="height: 10px"></el-col>
                    </el-row>

                  </el-row>

                  <div style="height:0px;"></div>
                  <el-row >
                    <div class="deailHead">
                      外出地点
                    </div>
                    <div class="deailBody" style="height:110px;">
                      <el-col :span="4" v-for="(areaName,index) in areaNameList.slice(areaA-1,areaB)" :key="1">
                        <div  :class="['choose', {choosed: areaName.ischoose}]" v-on:click="chooseArea(index)">
                          {{areaName.AreaName}}
                        </div>
                      </el-col>
                    </div>
                  </el-row>
                  <el-row >
                    <el-col :span="3" style="height: 10px"></el-col>
                    <el-col :span="18" >
                      <div class="pages">
                        <span class="pageControl"><img  v-on:click="areaBack()" src="../../assets/q1.png" alt=""/></span>
                        <span class="pagesText">{{areaNowPage}}/{{areaPages}}</span>
                        <span class="pageControl"><img  v-on:click="areaGo()" src="../../assets/q2.png" alt=""/></span>
                      </div>
                    </el-col>
                    <el-col :span="3" style="height: 10px"></el-col>
                  </el-row>
                </el-col>
                <el-col :span="1" style="height:10px;">
                <!--中间空隙-->
                </el-col>
                <el-col :span="11">
                  <el-row >
                    <div class="deailHead">
                      外出人员({{outCriminals.length}}人)
                    </div>
                    <div class="deailBody" style="height:269px;">

                      <el-col :span="4"  v-for="(criminal,index) in outCriminals.slice(outCriminalsA-1,outCriminalsB)" :key="1">
                        <div  class="criminal" v-on:click="delPerson(index)" >
                          <div style="height: 91px;width:90px;">
                          <img :src="criminal.Photo" width="98%" height="85" alt=""/>
                          </div>
                          <span class="criminalName">{{ criminal.CriminalName}}</span>
                        </div>
                      </el-col>
                    </div>
                  </el-row>
                  <el-row >
                    <el-col :span="3" style="height: 10px"></el-col>
                    <el-col :span="18" >
                      <div class="pages">
                        <span class="pageControl"><img src="../../assets/q1.png" v-on:click="outcriminalBack()" alt=""/></span>
                        <span class="pagesText">{{outNowPage}}/{{outPages}}</span>
                        <span class="pageControl"><img src="../../assets/q2.png" v-on:click="outcriminalGo()" alt=""/></span>
                      </div>
                    </el-col>
                    <el-col :span="3" style="height: 10px"></el-col>
                  </el-row>
                  <div style="height:0px;"></div>
                  <el-row >
                    <div class="deailHead">
                      陪同民警
                    </div>
                    <div class="deailBody" style="height:131px;">
                      <el-col :span="4"  v-for="(police,index) in outPolices.slice(outPoliceA-1,outPoliceB)" :key="1">
                        <div  class="criminal"  >
                          <div style="height: 91px;width: 90px;">
                            <img :src="police.Photo" width="98%" height="85" alt=""/>
                          </div>
                          <span class="criminalName">{{ police.PoliceName}}</span>
                        </div>
                      </el-col>
                    </div>
                  </el-row>
                  <el-row >
                    <el-col :span="3" style="height: 10px"></el-col>
                    <el-col :span="18" >
                      <!--<div class="pages">-->
                        <!--<span class="pageControl"><img src="../../assets/q1.png" v-on:click="outPoliceBack()" alt=""/></span>-->
                        <!--<span class="pagesText">{{outPoliceNowPage}}/{{outPolicePages}}</span>-->
                        <!--<span class="pageControl"><img src="../../assets/q2.png" v-on:click="outPoliceGo()" alt=""/></span>-->
                      <!--</div>-->
                    </el-col>
                    <el-col :span="3" style="height: 10px"></el-col>
                  </el-row>

                </el-col>
              </el-row>
            </div>
          </div>
          <div class="partsFoot">
            <div class="alertText">{{alertText}}</div>
            <div style="margin: 13px 2px;float: right">
              <div class="sure" v-on:click="submitOutRegister()">提交</div>
              <div class="sure" v-on:click="cancel()">取消</div>
            </div>
          </div>
        </div>

      </div>
    </el-col>
    <el-col :span="1"  style="height:10px"></el-col>
  </el-row>

</template>

<script>
  import { BasicUrl,IMG,ajaxUrl} from '../../config'

  export default {
    name: 'navheader',
    props:[
      'SocketAllData','criminalList','receiveDataMsgType20','receiveDataMsgType22','receiveDataMsgType27','policeList','receiveDataMsgType23','receiveDataMsgType26','canRouter'
    ],
    data () {
      return {
        orgTime:"0",
        seachNum:"0",
        searchModeShow:false,
        TimeShow:false,
        isShow1: true,
        isShow2: false,
        isB1: true,
        isB2: false,
        areaNameList:[],//外出地点
        areaPages:0,//外出地点总页数
        areaNowPage:1,//外出地点当前页
        areaListAll:0,//外出地点总数
        areaA:1,
        areaB:12,
        reasonList:[],// 外出事由
        reasonNowPage:1,// 外出事由当前页码
        reasonPages:1,// 外出事由总页码
        reasonListAll:0,
        reasonA:1,
        reasonB:30,
        isSuccess:0,

        outCriminals:[],//外出罪犯信息
        outPolices:[],//外出陪同民警信息
        outPages:1,//外出罪犯总页数
        outNowPage:1,//外出罪犯当前页
        outListAll:0,//外出罪犯总数
        outCriminalsA:1,
        outCriminalsB:12,
        outPolicePages:1,//外出民警总页数
        outPoliceNowPage:1,//外出民警当前页
        outPoliceListAll:0,//外出民警总数
        outPoliceA:1,
        outPoliceB:6,
        alertText:""

      }
    },
    methods: {
      /*搜索输入按键*/
      enterNum:function (Num) {
        if(Num=="-"){
          this.seachNum=""
        }else {
          this.seachNum= this.seachNum+Num
        }
      },
      setTime:function () {
          if(this.seachNum==""){
            this.seachNum= this.orgTime
          }
        this.searchModeShow=false

      },
      toggle1: function () {
        this.isShow1 = true
        this.isShow2 =false
        this.isB1 =  true
        this.isB2 = false
        this.recordPage=0
      },
      toggle2: function () {
        this.isShow1 = false
        this.isShow2 = true
        this.isB1 = false
        this.isB2 = true
      },
      chooseArea:function (dom) {
        for(var i=0;i< this.areaNameList.length;i++){
          this.areaNameList[i].ischoose=false
        }
        this.areaNameList[dom+this.areaA-1].ischoose=!this.areaNameList[dom+this.areaA-1].ischoose
      },
      chooseReason:function (dom,isNeed,time) {
        for(var i=0;i< this.reasonList.length;i++){
          this.reasonList[i].ischoose=false
        }
        this.reasonList[dom+this.reasonA-1].ischoose=!this.reasonList[dom+this.reasonA-1].ischoose

        if(isNeed=="1"){
          this.isNeed=true
          this.orgTime=time
          this.seachNum=time
          this.searchModeShow=true
          this.TimeShow=true

        }else {
          this.searchModeShow=false
          this.TimeShow=false

        }

      },
      areaGo:function () {
        if(this.areaNowPage<this.areaPages){
          this.areaNowPage=this.areaNowPage+1
          this.areaA=this.areaA+12
          this.areaB=this.areaB+12
        }else {
//          alert("已经最后一页了")
        }
      },
      areaBack:function () {
        if(this.areaNowPage==1){
//          alert("已经是第一页了")
        }else {
          this.areaNowPage=this.areaNowPage-1
          this.areaA=this.areaA-12
          this.areaB=this.areaB-12
        }

      },
      ReasonGo:function () {
          if(this.reasonNowPage<this.reasonPages){
            this.reasonNowPage=this.reasonNowPage+1
            this.reasonA=this.reasonA+30
            this.reasonB=this.reasonB+30
          }else {
//            alert("已经最后一页了")
          }
      },
      ReasonBack:function () {
        if(this.reasonNowPage==1){
//          alert("已经是第一页了")
        }else {
          this.reasonNowPage=this.reasonNowPage-1
          this.reasonA=this.reasonA-30
          this.reasonB=this.reasonB-30
        }

      },
      outcriminalGo:function () {
        if(this.outNowPage<this.outPages){
          this.outNowPage=this.outNowPage+1
          this.outCriminalsA=this.outCriminalsA+12
          this.outCriminalsB=this.outCriminalsB+12
        }else {
//          alert("已经最后一页了")
        }
      },
      outcriminalBack:function () {
        if(this.outNowPage==1){
//          alert("已经是第一页了")
        }else {
          this.outNowPage=this.outNowPage-1
          this.outCriminalsA=this.outCriminalsA-12
          this.outCriminalsB=this.outCriminalsB-12
        }
      },
      outPoliceGo:function () {
        if(this.outPoliceNowPage<this.outPolicePages){
          this.outPoliceNowPage=this.outPoliceNowPage+1
          this.outPoliceA=this.outPoliceA+6
          this.outPoliceB=this.outPoliceB+6
        }else {
//          alert("已经最后一页了")
        }
      },
      outPoliceBack:function () {
        if(this.outPoliceNowPage==1){
//          alert("已经是第一页了")
        }else {
          this.outPoliceNowPage=this.outPoliceNowPage-1
          this.outPoliceA=this.outPoliceA-6
          this.outPoliceB=this.outPoliceB-6
        }
      },
      firstWs:function () {
        var vm=this
        var send1 = {
          Header: {
            MsgID:"201501260000000035",
            MsgType:20
          },
          Body: JSON.stringify({
            OrgID : localStorage.getItem('OrgID'),
            DoorID : localStorage.getItem('DoorID'),
            AreaID : localStorage.getItem('AreaID'),
            RegType:2603
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
          data:JSON.stringify(send1),
          success: function (result) {
            if(result.RET==1){
              vm.isSuccess=1

            }else {
              vm.isSuccess=0
            }
          },
          complete: function (XHR, TS) {
            XHR = null;  //回收资源
          }
        })

      },
      cancel:function () {
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
            if(result.RET==1){
              vm.alertText="取消成功"
              vm.$emit('canRouterChange')
              localStorage.setItem("moveTypes","0")

              setTimeout(function () {
                vm.alertText=""
                vm.$router.push({ path: '/' })
              },2000)


            }else {
              vm.alertText="取消失败"
              vm.$emit('canRouterChange')
              setTimeout(function () {
                vm.alertText=""
              },2000)

            }
          },
          complete: function (XHR, TS) {
            XHR = null;  //回收资源
          }
        })

      },
      delPerson:function (index) {
          var vm = this
        this.$confirm('确定要删除该人员?', '提示', {
          confirmButtonText: '删除',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          var sendDelPerson = {
            Header: {
              MsgID:"201501260000000031",
              MsgType:21
            },
            Body: JSON.stringify({
              OrgID : localStorage.getItem('OrgID'),
              DoorID : localStorage.getItem('DoorID'),
              PeopleID :vm.outCriminals[index+vm.outCriminalsA-1]["CriminalID"]
            })
          }
          $.ajax({
            type: "get",
            contentType: "application/json; charset=utf-8",
            dataType: "jsonp",
            jsonp: "callback",
            async: false,
            url: ajaxUrl,
            data:JSON.stringify(sendDelPerson),
            success: function (result) {
              if(result.RET==1){
                vm.outCriminals.splice(index+vm.outCriminalsA-1,1)
              }else {
                vm.$message("删除失败")
//                vm.alertText="删除失败"
//                setTimeout(function () {
//                  vm.alertText=""
//                },2000)
              }
            },
            complete: function (XHR, TS) {
              XHR = null;  //回收资源
            }
          })

        }).catch(() => {

        });
//        var r=confirm("确定要删除该人员？");
//        if (r==true)
//        {
//
////          alert(vm.outCriminals[index+vm.outCriminalsA-1]["CriminalID"])
//
//          var sendDelPerson = {
//            Header: {
//              MsgID:"201501260000000031",
//              MsgType:21
//            },
//            Body: JSON.stringify({
//              OrgID : localStorage.getItem('OrgID'),
//              DoorID : localStorage.getItem('DoorID'),
//              PeopleID :vm.outCriminals[index+vm.outCriminalsA-1]["CriminalID"]
//            })
//          }
//          $.ajax({
//            type: "get",
//            contentType: "application/json; charset=utf-8",
//            dataType: "jsonp",
//            jsonp: "callback",
//            async: false,
//            url: ajaxUrl,
//            data:JSON.stringify(sendDelPerson),
//            success: function (result) {
//              if(result.RET==1){
//                  vm.outCriminals.splice(index+vm.outCriminalsA-1,1)
//              }else {
//                vm.alertText="删除失败"
//                setTimeout(function () {
//                  vm.alertText=""
//                },2000)
//
//              }
//            },
//            complete: function (XHR, TS) {
//              XHR = null;  //回收资源
//            }
//          })
//
//        }

      },

      submitOutRegister:function () {
        var vm=this
        var Criminals="";
        var Polices="";
        var Areas="";
//      var Reason=[];
        var Reason='';

        for (var i=0;i<vm.outCriminals.length;i++){
//          Criminals=Criminals+vm.outCriminals[i]["CriminalID"]
          if(i==0){
            Criminals=vm.outCriminals[i]["CriminalID"]
          }else {
            Criminals=Criminals+","+vm.outCriminals[i]["CriminalID"]
          }
        }
        for (var i=0;i<vm.outPolices.length;i++){
//          Polices.push(vm.outPolices[i]["PersonID"])
          if(i==0){
            Polices=vm.outPolices[i]["PersonID"]
          }else {
            Polices=Polices+","+vm.outPolices[i]["PersonID"]
          }

        }
        for (var i=0;i<vm.areaNameList.length;i++){
            if(vm.areaNameList[i]["ischoose"]){
              Areas=vm.areaNameList[i]["FlnkID"]
            }
        }
        for (var i=0;i<vm.reasonList.length;i++){
          if(vm.reasonList[i]["ischoose"]){
//          Reason.push(vm.reasonList[i]["Description"])
            Reason=vm.reasonList[i]["DictCodeName"]
          }
        }
        if(Areas==""||Reason==""||Criminals==""){
          vm.alertText="外出事由和外出地点必须选择，外出人员也不能为空"
          setTimeout(function () {
            vm.alertText=""
          },2000)
        }else {

          var sendOutRegister = {
            Header: {
              MsgID:"201501260000000031",
              MsgType:23
            },
            Body: JSON.stringify({
              OrgID : localStorage.getItem('OrgID'),
              DoorID : localStorage.getItem('DoorID'),
              IsSpectialMove:vm.TimeShow,
              UnhandledTime:vm.seachNum,
//            AreaID : localStorage.getItem('AreaID'),
              Criminals:Criminals,
              Polices:Polices,
              Reason:Reason,
              Areas:Areas
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
            data:JSON.stringify(sendOutRegister),
            success: function (result) {
              localStorage.setItem("moveTypes","0")
              if(result.RET==1){
                vm.$emit('canRouterChange')
                vm.alertText="提交成功"
                setTimeout(function () {
                  vm.alertText=""
                  vm.$router.push({ path: '/' })
                },2000)
              }else {
                vm.$emit('canRouterChange')
                vm.alertText="提交失败"
                setTimeout(function () {
                  vm.alertText=""
                },2000)
              }
            },
            complete: function (XHR, TS) {
              XHR = null;  //回收资源
            }
          })
        }

      }

    },
    mounted () {
      /* Coding By YanM */

      /* Coding By YanM */
      /* Coding By Qianjf */
      var vm = this
      localStorage.setItem("placemanID","0")
     var outPlice= setInterval(function () {
        if(localStorage.getItem("placemanID")==0){
            /*民警还未刷卡*/
        }else if(localStorage.getItem("placemanID")==1){
          /* 点击登录框关闭按钮停止检测民警登录情况*/
          clearInterval(outPlice)
        }else{

          localStorage.setItem("moveTypes","2")//1为进出工，2为临时外出登记，3为卡绑定
          vm.firstWs()
          clearInterval(outPlice)
//          console.log(vm.policeList)
          var Polices={}
          Polices["PersonID"]=localStorage.getItem("placemanID")
          Polices["ischoose"]=false
          Polices["PoliceName"]=vm.policeList[0][localStorage.getItem("placemanID")]["PoliceName"]
          Polices["Photo"]=vm.policeList[0][localStorage.getItem("placemanID")]["Photo"]
          vm.outPolices.push(Polices)
        }
      },500)
//      发送人员流动状态  2603临时外出
//      setInterval(function () {
//          if(vm.isSuccess==0){
//            vm.firstWs()
//          }
//      },1000)

//      获取外出登记的人员明细
    setInterval(function () {
        if(vm.isSuccess==1){
          var send2 = {
            Header: {
              MsgID:"201501260000000035",
              MsgType:22
            },
            Body: JSON.stringify({
              OrgID : localStorage.getItem('OrgID'),
              DoorID : localStorage.getItem('DoorID'),
            })
          }
          var send27 = {
            Header: {
              MsgID:"201501260000000035",
              MsgType:27
            },
            Body: JSON.stringify({
              OrgID : localStorage.getItem('OrgID'),
              DoorID : localStorage.getItem('DoorID'),
            })
          }
          //发送数据
          if(vm.ws.readyState == WebSocket.OPEN){
            vm.ws.send(JSON.stringify(send2))
          }
          if(vm.ws.readyState == WebSocket.OPEN){
            vm.ws.send(JSON.stringify(send27))
          }
          /*外出登记罪犯信息*/
          var receiveData = vm.receiveDataMsgType22
          if(receiveData!=""&&receiveData!=null){
            var outCriminal=[] //外出罪犯
            for (var i=0;i<receiveData.length;i++){
                var Criminal=receiveData[i]
                    Criminal["ischoose"]=false
                    Criminal["CriminalName"]=vm.criminalList[0][Criminal["CriminalID"]]["CriminalName"]
                    Criminal["Photo"]=vm.criminalList[0][Criminal["CriminalID"]]["Photo"]
                    outCriminal.push(Criminal)
                    vm.outCriminals=outCriminal
                    vm.outPages=Math.ceil(vm.outCriminals.length/12)==0?1:Math.ceil(vm.outCriminals.length/12)
            }
          }
          /*陪同民警信息*/
          var receiveDataP = vm.receiveDataMsgType27
          if(receiveDataP!=""&&receiveDataP!=null){
            var outPolice=[] //陪同民警
            for (var i=0;i<receiveDataP.length;i++){
              var Polices=receiveDataP[i]
              Polices["ischoose"]=false
              Polices["PoliceName"]=vm.policeList[0][Polices["PersonID"]]["PoliceName"]
              Polices["Photo"]=vm.policeList[0][Polices["PersonID"]]["Photo"]
              outPolice.push(Polices)
              vm.outPolices=outPolice
              vm.outPolicePages=Math.ceil(vm.outPolices.length/6)==0?1:Math.ceil(vm.outPolices.length/6)
            }
          }
        }
      },1000)

//      获取外出地点
      $.ajax({
        type: "get",
        contentType: "application/json; charset=utf-8",
        dataType: "jsonp",
        jsonp: "callback",
        async: false,
        data:{"OrgID":localStorage.getItem("OrgID")},
        url:  BasicUrl+'Move/GetOutAreaList' + "?callback=?",
        success: function (result) {
          if(result!=""&&result!=null){
            vm.areaListAll=result.length
              vm.areaPages=Math.ceil(result.length/12)==0?1:Math.ceil(result.length/12)
            for (var i=0;i<result.length;i++){
               result[i]["ischoose"]=false
            }
            vm.areaNameList=result
          }

        },
        error: function (err) {
//          alert("请求异常")
        },
        complete: function (XHR, TS) {
          XHR = null;  //回收资源
        }
      });
//      获取外出事由
      $.ajax({
        type: "get",
        contentType: "application/json; charset=utf-8",
        dataType: "jsonp",
        jsonp: "callback",
        async: false,
        data:{"OrgID":localStorage.getItem("OrgID")},
        url: BasicUrl+'Move/GetMoveReasonList' + "?callback=?",
        success: function (result) {
          if(result!=""&&result!=null){
            vm.reasonListAll=result.length
            vm.reasonPages=Math.ceil(result.length/30)==0?1:Math.ceil(result.length/30)
            for (var i=0;i<result.length;i++){
              result[i]["ischoose"]=false
            }
            vm.reasonList=result

             console.log(result)
          }

        },
        error: function (err) {
//          alert("请求异常")
        },
        complete: function (XHR, TS) {
          XHR = null;  //回收资源
        }
      });
      /* Coding By Qianjf */

    }

  }
</script>

<style>
  .li3_parts .tab{
    width: 155px;
    height: 40px;
    background: #004bdc;
    font-size: 18px;
    text-align: center;
    float: left;
    color:white;
    line-height: 38px;
  }
  .li3_parts .tabing{
    background: white;
    font-size: 18px;
    color: #004bdc;
    text-align: center;
  }
  .home{
    height: 836px;
  }
  .li3_parts {
    height: 740px;
    margin: 0px auto;
  }
  .li3_parts .deailHead{
    width: 100%;
    height: 32px;
    background: rgba(0, 126, 202, 0.25);
    color: blue;
    text-align: left;
    text-indent: 28px;
    font-size: 19px;
    line-height: 33px;
  }
  li3_parts .deailBody{

  }
  .criminalImg{
    height: 91px;
    width: 89px;
  }

  .li3_parts .partsBody {
    width: 100%;
    height: 710px;
    background: white;
  }
  .li3_parts .partsFoot{
    width: 100%;
    height: 84px;
    /*margin-top: -48px;*/
    background: #c5cfdb;

  }
  .li3_parts .bodyCon{
    height: 592px;
    padding: 20px;

  }
  .li3_parts .pages{
    width: 273px;
    height: 50px;
    margin: 12px auto;
  }
  .li3_parts .pageControl{

  }
  .li3_parts .pagesText {
    font-size: 24px;
    display: inline-block;
    width: 155px;
    text-align: center;
  }
  .li3_parts .sure{
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



  .li3_parts .criminal{
    width: 84%;
    height: 110px;
    float: left;
    background: #0048d9;
    margin: 6px 6px;
    text-align: center;
    padding: 2px;
  }
  .li3_parts .criminal img{
    border: 1px solid white;
  }
  .li3_parts .criminalName {
    font-size: 12px;
    color: white;
    display: inline-block;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    width: 99%;
  }
  .li3_parts .bodyHead{
    width: 100%;
    height: 46px;
    background: #c5cfdb;
  }
  .li3_parts .title{
    font-size: 25px;
    font-weight: 800;
    color: #1d68e8;
    text-shadow: 2px 2px 2px #fff;
    line-height: 45px;
    text-indent: 15px;
    float: left;

  }
  .li3_parts .titleDescribe{
    line-height: 45px;
    font-size: 20px;
    width: 856px;
    text-align: left;
  }
  .tab2 .bodyCon{
    height: 600px;
  }


  .choose{
    border: 1px solid blue;
    height: 37px;
    line-height: 35px;
    margin: 6px 5px;
    color: blue;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .choosed{
    border: 1px solid blue;
    height: 37px;
    line-height: 35px;
    margin: 6px 5px;
    color: white;
    background: blue;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .searchInput::-webkit-input-placeholder{
    color: #fff;
  }
  .searchMode{
    position: absolute;
    z-index: 9999999;
    background: #a2bcdb;
    border-radius: 7px;
    margin: 195px 545px;
    height: 351px;
    width: 437px;
  }
  .searchInput{
    width: 353px;
    height: 47px;
    margin: 6px 0px;
    font-size: 30px;
    text-align: center;
    background: #697a8f;
    color: white;
  }

  .searchBottons{
    width: 383px;
    height: 10px;
    margin: 0px auto;

  }
  .listX{
    width: 100%;

  }
  .listY{
    width: 76px;
    height: 49px;
    float: left;
    border: 1px solid blue;
    margin: 8px 8px;
    font-size: 30px;
    line-height: 51px;
    background: #3f73fb;
    color: white;
    border-radius: 8px;
    box-shadow: 1px 1px 4px -2px;

  }
  .listY:active{
    width: 76px;
    height: 49px;
    float: left;
    border: 1px solid blue;
    margin: 8px 8px;
    font-size: 30px;
    line-height: 51px;
    border-radius: 8px;
    box-shadow: 1px 1px 4px 4px;
  }

</style>
