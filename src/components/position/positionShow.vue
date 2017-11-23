<template>
  <!--<div class="navheader">人员清点</div>-->
  <el-row class="menu_title_wrap home">
    <el-col :span="1" style="height:10px"></el-col>
    <el-col :span="22">
      <div class="li4_parts">
        <div class="tab1">
          <div class="partsBody">
            <div class="bodyHead">
              <div class="title">人员分布</div>
            </div>
            <div class="bodyCon">
              <!--键盘模块Star-->
              <div class="searchMode" v-show="searchModeShow">
                <input class="searchInput" v-model="seachNum" placeholder="请输入罪犯编号" type="text">
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
                  <div class="listY" v-on:click="enterNum('Q')">Q</div>
                  <div class="listY" v-on:click="enterNum('W')">W</div>
                  <div class="listY" v-on:click="enterNum('E')">E</div>
                  <div class="listY" v-on:click="enterNum('R')">R</div>
                  <div class="listY" v-on:click="enterNum('T')">T</div>
                  <div class="listY" v-on:click="enterNum('Y')">Y</div>
                  <div class="listY" v-on:click="enterNum('U')">U</div>
                  <div class="listY" v-on:click="enterNum('I')">I</div>
                  <div class="listY" v-on:click="enterNum('O')">O</div>
                  <div class="listY" v-on:click="enterNum('P')">P</div>
                  <div  class="listY" style="width: 26px;border: none;box-shadow: none;background: none"></div>
                  <div class="listY" v-on:click="enterNum('A')">A</div>
                  <div class="listY" v-on:click="enterNum('S')">S</div>
                  <div class="listY" v-on:click="enterNum('D')">D</div>
                  <div class="listY" v-on:click="enterNum('F')">F</div>
                  <div class="listY" v-on:click="enterNum('G')">G</div>
                  <div class="listY" v-on:click="enterNum('H')">H</div>
                  <div class="listY" v-on:click="enterNum('J')">J</div>
                  <div class="listY" v-on:click="enterNum('K')">K</div>
                  <div class="listY" v-on:click="enterNum('L')">L</div>
                  <div class="listY" v-on:click="enterNum('-')"style="font-size: 23px;width:77px;">清除</div>
                  <div class="listY" v-on:click="enterNum('Z')">Z</div>
                  <div class="listY" v-on:click="enterNum('X')">X</div>
                  <div class="listY" v-on:click="enterNum('C')">C</div>
                  <div class="listY" v-on:click="enterNum('V')">V</div>
                  <div class="listY" v-on:click="enterNum('B')">B</div>
                  <div class="listY" v-on:click="enterNum('N')">N</div>
                  <div class="listY" v-on:click="enterNum('M')">M</div>
                  <div class="listY" style="font-size: 23px;width: 170px;" v-on:click="searchSub()">搜索</div>
                  <div class="listX">
                    <div class="listY" style="font-size: 23px;width: 926px;" v-on:click="searchModeShow=false">关闭</div>
                  </div>
                </div>
              </div>
              <!--键盘模块End-->
              <el-row >
                <el-col :span="5">
                  <el-row >
                    <div class="deailBody">
                      <div v-for="(item,index) in allMapLists" v-on:click="selectMap(index)" :class="['mapPic',{chosedMap:item.status}]" :key="12" >
                        <img :src="item.MapUrl" width="100%" alt="">
                        <span>{{item.AreaName}}</span>
                      </div>

                    </div>
                  </el-row>
                </el-col>
                <el-col :span="1" style="height: 1px"></el-col>
                <el-col :span="17">
                  <div style="height:0px;"></div>
                  <el-row >
                    <!--<el-row style="overflow: hidden;height: 632px;margin: 18px 4px;width:1025px;">-->
                    <el-row style="overflow: hidden;height: 540px;margin: 18px 4px;width:1025px;">

                      <div class="map" v-on:click="closeTip()">
                        <!--{{criminalLists}}-->
                        <img :src="positionMap" id="positionMap" width="" alt="" ref="abc">
                        <!--<img :src="mapPhoto" alt="" ref="abc">-->
                        <div  v-for="(item,index) in criminalLists" v-show="item.pointShows" :class="['point', {pointed: true}]"  @click.stop="select(index)"  :style="{top:item.CriminalY+'px',left:item.CriminalX+'px'}" >
                          <img :src="item.pointImg" alt="">
                          <div style="width: 100px;text-align: center;position: absolute;margin:-61px -42px;">{{item.Name}}</div>
                          <div class="pointTop" v-show="item.status">
                            <el-col :span="10">
                              <img :src="item.CriminalPhoto" style="height: 140px;width: 100px;" alt="">
                            </el-col>
                            <el-col :span="14" style="text-align: left;line-height: 30px;">
                              姓名：{{item.Name}}<br/>
                              番号：{{item.PersonID}}<br/>
                              监区：{{item.OrgName}}<br/>
                              管理等级：{{item.ManageLevelName}}<br/>
                            </el-col>
                            <div style="width: 0px;height: 0px;border-left: 18px solid transparent;border-right: 16px solid transparent;border-top: 13px solid #022171;font-size: 0;line-height: 0;margin: 142px 42px;"></div>
                          </div>
                        </div>


                        <div class="personGroups" v-for="(item,index) in criminalGroups" :style="{top:item.CriminalY+'px',left:item.CriminalX+'px'}" @click.stop="selectGroups(index)" >
                          {{item.posMenu.length}}
                          <div style=" margin: -67px 37px;" v-show="item.status" >
                            <div class="personGroup" v-for="(item,personIndex) in item.posMenu[0]" @click.stop="selectPerson(index,personIndex)">
                              <div class="personName">
                                {{item.Name}}
                                <div class="pointTop" style="margin: -83px 158px;font-size: 16px" v-show="item.status">
                                  <el-col :span="10">
                                    <img :src="item.CriminalPhoto" style="height: 140px;width: 100px;" alt="">
                                  </el-col>
                                  <el-col :span="14" style="text-align: left;line-height: 30px;">
                                    姓名：{{item.Name}}<br/>
                                    番号：{{item.PersonID}}<br/>
                                    监区：{{item.OrgName}}<br/>
                                    管理等级：{{item.ManageLevelName}}<br/>
                                  </el-col>
                                  <div style="width: 0px;height: 0px;border-left: 18px solid transparent;border-right: 16px solid transparent;border-top: 13px solid #022171;font-size: 0;line-height: 0;margin: 68px -20px;"></div>
                                </div>
                              </div>
                            </div>

                          </div>
                        </div>
                      </div>
                    </el-row>
                  </el-row>
                  <!--<div>-->
                  <!--<span style="font-size: 20px;color: black;font-weight: 700;">当前区域：</span>-->
                  <!--<span style="color:#d98900;font-size: 17px;font-weight:700"><span style="display: inline-block;width: 15px;height: 15px;border-radius: 100px;background: #d98900"></span>&nbsp;宽管：{{KGL}}人&nbsp;&nbsp;</span>　-->
                  <!--<span style="color:#ff0000;font-size: 17px;font-weight:700"><span style="display: inline-block;width: 15px;height: 15px;border-radius: 100px;background: #ff0000"></span>&nbsp;严管：{{YGL}}人&nbsp;&nbsp;</span>　-->
                  <!--<span style="color:#00b322;font-size: 17px;font-weight:700"><span style="display: inline-block;width: 15px;height: 15px;border-radius: 100px;background: #00b322"></span>&nbsp;普管：{{PGL}}人&nbsp;&nbsp;</span>　-->
                  <!--<span style="color:#fa00d4;font-size: 17px;font-weight:700"><span style="display: inline-block;width: 15px;height: 15px;border-radius: 100px;background: #fa00d4"></span>&nbsp;考察：{{KCL}}人&nbsp;&nbsp;</span>-->
                  <!--<span style="color:#020508;font-size: 17px;font-weight:700"><span style="display: inline-block;width: 15px;height: 15px;border-radius: 100px;background: rgba(73,158,250,0)"></span>&nbsp;总共人数：{{KCL+PGL+YGL+KGL}}人&nbsp;&nbsp;</span>-->
                  <!--</div>-->

                  <div class="personListShows" style="overflow: hidden">
                    <div class="showList" style=" overflow: auto;height: 137px;width: 1057px;">
                      <div  :class="['personShow', {personShowed: item.status}]"  v-for="(item,index) in criminalLists" v-on:click="choosePerson(index)" >
                        <div class="personImg" style="width: 72px;height: 90px"><img :src="item.CriminalPhoto" style="height: 90px;width: 72px;" alt=""></div>
                        <div class="perName">{{item.Name}}</div>
                      </div>
                    </div>
                  </div>
                </el-col>
                <el-col :span="1" style="height:10px;">
                  <!--中间空隙-->
                  <div>
                    <div class="slc" v-on:click="changeSize('+')"><img src="../../assets/bigger.png" alt=""></div>
                    <div class="slc" v-on:click="changeSize('-')"><img src="../../assets/smaller.png" alt=""></div>
                    <div class="slc" v-on:click="searchModeShow=true;seachNum=''"><img src="../../assets/search.png" alt=""></div>
                  </div>
                </el-col>
              </el-row>
            </div>
          </div>
        </div>
      </div>
    </el-col>
    <el-col :span="1"  style="height:10px"></el-col>
  </el-row>

</template>

<script>
  import { BasicUrl,IMG,ajaxUrl,MapUrl } from '../../config'
  import { ajax } from '../../assets/ajaxWebApiMethod'
  var scaleNum;

  export default {
    name: 'navheader',
    props:[
      'mapList',
      'criminalList',
    ],

    data () {
      return {
        mergePoints:[],
        prisonSelectText:"",
        getGroupSet:'',
        isOpenGroup:1,//重合人员是否展开
        criminalGroups:'',
        ipImg:"",
        searchModeShow:false,
        seachNum:"",
        personBand:[],
        mapPhoto:"",
        getMapAct:0,
        getSearch:0,
        criminalLists:[],
        criminalGroupIDs:localStorage.getItem("criminalGroupIDs"),
        allMapLists:[],
        criminalsituationLsit:'',
        positionMap:'',
        MapID:'',
        KGL:0,
        YGL:0,
        PGL:0,
        KCL:0


      }
    },
    methods: {
      /*关闭气泡详情*/
      closeTip:function () {
        let vm = this
        vm.isOpenGroup=1
        for(let i = 0; i<vm.criminalLists.length; i++){
          vm.criminalLists[i].status = false
        }

        for(var i = 0; i<vm.criminalGroups.length; i++){
          vm.criminalGroups[i].status = false
          for(var j = 0; j<vm.criminalGroups[i].posMenu[0].length; j++){
            vm.criminalGroups[i].posMenu[0][j].status = false
          }

        }

      },
      /*底部选择*/
      choosePerson:function (index) {
        for(let i=0;i<this.criminalLists.length;i++){
          this.criminalLists[i].status=false
        }
        this.criminalLists[index].status=true
        this.criminalLists[index].pointShows=true


      },
      /*搜索罪犯*/
      searchSub:function () {
        let vm=this
        let sendMessage  = {
          Header: {
            MsgID:"201501260000000035",
            MsgType:12
          },
          Body: JSON.stringify({
            PSID:vm.seachNum,
            Status:0,
            PSType:"2002"
          })
        }
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(sendMessage),
          success: function (result) {
            if(result.MapID !="00000000-0000-0000-0000-000000000000"){
              clearInterval(vm.getMapAct)
              clearInterval(vm.getGroupSet)
              vm.criminalGroups=[]
              vm.criminalLists=[]
              vm.positionMap=MapUrl+vm.mapList[0][result.MapID].MapUrl
              var ManageLevels=vm.personBand[0][result.PSID.toLowerCase()].ManageLevel
              var pointImg="";
              if(ManageLevels=="4201"){
                pointImg=vm.ipImg+"KG.png"
              }else if(ManageLevels=="4202"){
                pointImg=vm.ipImg+"PG.png"
              }else if(ManageLevels=="4203"){
                pointImg=vm.ipImg+"YG.png"
              }else if(ManageLevels=="4204"){
                pointImg=vm.ipImg+"KC.png"
              }
              vm.criminalLists.push({
                PSID:result.PSID.toLowerCase(),
                Name:vm.personBand[0][result.PSID.toLowerCase()].Name,
                PersonID:vm.personBand[0][result.PSID.toLowerCase()].PersonID,
                OrgName:vm.personBand[0][result.PSID.toLowerCase()].OrgName,
                ManageLevelName:vm.personBand[0][result.PSID.toLowerCase()].ManageLevelName,
                CriminalPhoto:vm.personBand[0][result.PSID.toLowerCase()].Photo,
                status:true,
                pointShows:true,
                ManageLevel:vm.personBand[0][result.PSID.toLowerCase()].ManageLevel,
                CriminalX:result.X,
                CriminalY:result.Y,
                pointImg:pointImg
              })
              vm.getSearch=setInterval(function () {
                vm.searchPoint()
              },2000)
              vm.searchModeShow=false

            }else {
              vm.$message('查无此人信息，请确认罪犯编号是否有误')
            }


          },
          complete: function (XHR) {
            XHR = null;  //回收资源

          }
        });

      },
      /*搜索罪犯点位刷新*/
      searchPoint:function () {
        let vm=this
        let sendMessage  = {
          Header: {
            MsgID:"201501260000000035",
            MsgType:12
          },
          Body: JSON.stringify({
            PSID:vm.seachNum,
            Status:0,
            PSType:"2002"
          })
        }
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(sendMessage),
          success: function (result) {
            if(result.MapID !="00000000-0000-0000-0000-000000000000"){
              vm.positionMap=MapUrl+vm.mapList[0][result.MapID].MapUrl
              var ManageLevels=vm.personBand[0][result.PSID.toLowerCase()].ManageLevel
              var pointImg="";
              if(ManageLevels=="4201"){
                pointImg=vm.ipImg+"KG.png"
              }else if(ManageLevels=="4202"){
                pointImg=vm.ipImg+"PG.png"
              }else if(ManageLevels=="4203"){
                pointImg=vm.ipImg+"YG.png"
              }else if(ManageLevels=="4204"){
                pointImg=vm.ipImg+"KC.png"
              }

              vm.criminalLists[0].Name=vm.personBand[0][result.PSID.toLowerCase()].Name
              vm.criminalLists[0].PersonID=vm.personBand[0][result.PSID.toLowerCase()].PersonID
              vm.criminalLists[0].OrgName=vm.personBand[0][result.PSID.toLowerCase()].OrgName
              vm.criminalLists[0].ManageLevelName=vm.personBand[0][result.PSID.toLowerCase()].ManageLevelName
              vm.criminalLists[0].CriminalPhoto=vm.personBand[0][result.PSID.toLowerCase()].Photo
              vm.criminalLists[0].ManageLevel=vm.personBand[0][result.PSID.toLowerCase()].ManageLevel
              vm.criminalLists[0].pointImg=pointImg
              vm.criminalLists[0].CriminalX=result.X
              vm.criminalLists[0].CriminalY=result.Y
            }

          },
          complete: function (XHR) {
            XHR = null;  //回收资源
          }
        });

      },
      /*搜索输入按键*/
      enterNum:function (Num) {
        if(Num=="-"){
          this.seachNum=""
        }else {
          this.seachNum= this.seachNum+Num
        }
      },
      /* 选择互监组成员 */
      select:function (index) {
        let vm = this
        for(let i = 0; i<vm.criminalLists.length; i++){
          vm.criminalLists[i].status = false
        }
        vm.criminalLists[index].status = true
        vm.criminalsituationLsit= vm.criminalLists[index].CriminalID
        if(vm.criminalLists[index].MapUrl==null){

        }else {
          vm.mapPhoto= vm.criminalLists[index].MapUrl
          vm.criminalLists[index].pointStatus=true
          for (var k=0 ;k<vm.criminalLists.length;k++){
            if(vm.criminalLists[k].MapUrl !=null){
              if(vm.mapPhoto !=vm.criminalLists[k].MapUrl){
                vm.criminalLists[k].pointStatus=false
              }
            }
          }
        }
      },
      /*选择重叠组*/
      selectGroups:function (index) {
        let vm = this
        vm.isOpenGroup=0
        setTimeout(function () {
          vm.closeTip()
        },8000)
        for(let i = 0; i<vm.criminalGroups.length; i++){
          vm.criminalGroups[i].status = false
        }
        vm.criminalGroups[index].status = true

      },
      /*选择重叠组*/
      selectPerson:function (index,personIndex) {
        let vm = this
        for(let i = 0; i<vm.criminalGroups[index].posMenu[0].length; i++){
          vm.criminalGroups[index].posMenu[0][i].status = false
        }
        vm.criminalGroups[index].posMenu[0][personIndex].status = true
      },
      /*获取新的绑卡人基础数据*/
      getNewData:function () {
        /* 人员分布人员基础信息 */
        let vm = this;
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          data: {OrgID: localStorage.getItem('OrgID')},
          url:  BasicUrl+'/CriminalCnt/GetCardBindPersonList',
          success: function (result) {

            //所有罪犯信息缓存(哈希，便于快速查找缓存中的罪犯详细信息)
            var person_hash = new Array();
            for(var i=0;i<result.length;i++){
              person_hash[result[i].PSID.toLowerCase()] = {
                FlnkID:result[i].FlnkID,
                PersonID:result[i].PersonID,
                Name:result[i].Name,
                Pinyin:result[i].Pinyin,
                PSType:result[i].PSType,
                ManageLevel:result[i].ManageLevel,
                OrderIndex:result[i].OrderIndex,
                ManageLevelName:result[i].ManageLevelName,
                ManageLevelColor:result[i].ManageLevelColor,
                OrgID:result[i].OrgID,
                OrgName:result[i].OrgName,
                Photo:MapUrl+result[i].Photo,
                PSID:result[i].PSID.toLowerCase(),
                IC:result[i].IC
              };
            }
            vm.personBand[0]=person_hash
          },
          complete: function (XHR, TS) {
            XHR = null;  //回收资源

          }
        });
      },

      selectMap:function (index) {
        let vm = this
        vm.KGL=0
        vm.YGL=0
        vm.PGL=0
        vm.KCL=0

        for(let i = 0; i<vm.allMapLists.length; i++){
          vm.allMapLists[i].status = false
        }
        vm.allMapLists[index].status= true
        vm.positionMap=vm.allMapLists[index].MapUrl
        vm.MapID=vm.allMapLists[index].MapFlnkID
        vm.prisonSelectText=vm.allMapLists[index].AreaName
        vm.pointS()
      },
      /*缩放地图*/
      changeSize:function (type) {
        if(type=="+"){
          scaleNum=scaleNum+0.1
        }else if(type=="-"){
          if(scaleNum>0.2){
            scaleNum=scaleNum-0.1
          }
        }else {
//            scaleNum=1
        }
        $(".map").css("transform", "scale("+scaleNum+")");
        $(".map").css("-ms-transform-origin", "center center");
        $(".map").css("transform-origin", "center center");
        $(".map").css("-webkit-transform-origin", "center center");
//        $(".map").css("transform-origin", "0 0");
//        $(".map").css("-webkit-transform-origin", "0 0");
        $(".map").css("-moz-transform-origin", "center center");
        $(".map").css("-o-transform-origin", "center center");
      },
      /* 地图定位 */
      pointS:function () {
        let vm = this
        clearInterval(vm.getSearch)
        vm.getNewData()
        vm.criminalLists=[]
        let sendMessage  = {
          Header: {
            MsgID:"201501260000000035",
            MsgType:10
          },
          Body: JSON.stringify({
            MapID:vm.MapID,
            OrgID:localStorage.getItem("OrgID").toLowerCase(),
            ManageLevel:0,
            AreaID:"00000000-0000-0000-0000-000000000000",
            PSType:"2002"
          })
        }
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(sendMessage),
          success: function (result) {

            for (let i=0;i<result.length;i++){
              var ManageLevels=vm.personBand[0][result[i].PSID.toLowerCase()].ManageLevel
              var pointImg="";
              if(ManageLevels=="4201"){
                pointImg=vm.ipImg+"KG.png"
              }else if(ManageLevels=="4202"){
                pointImg=vm.ipImg+"PG.png"
              }else if(ManageLevels=="4203"){
                pointImg=vm.ipImg+"YG.png"
              }else if(ManageLevels=="4204"){
                pointImg=vm.ipImg+"KC.png"
              }

              vm.criminalLists.push({
                pointShows:true,
                PSID:result[i].PSID.toLowerCase(),
                Name:vm.personBand[0][result[i].PSID.toLowerCase()].Name,
                PersonID:vm.personBand[0][result[i].PSID.toLowerCase()].PersonID,
                OrgName:vm.personBand[0][result[i].PSID.toLowerCase()].OrgName,
                ManageLevelName:vm.personBand[0][result[i].PSID.toLowerCase()].ManageLevelName,
                CriminalPhoto:vm.personBand[0][result[i].PSID.toLowerCase()].Photo,
                status:false,
                ManageLevel:vm.personBand[0][result[i].PSID.toLowerCase()].ManageLevel,
                CriminalX:result[i].X,
                CriminalY:result[i].Y,
                pointImg:pointImg
              })
            }
            let KG=[]
            let YG=[]
            let PG=[]
            let KC=[]
            for (let j=0;j<vm.criminalLists.length;j++){
              if(vm.criminalLists[j].ManageLevel=="4201"){
                KG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4202"){
                PG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4203"){
                YG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4204"){
                KC.push(vm.criminalLists[j])
              }
              vm.KGL=0
              vm.YGL=0
              vm.PGL=0
              vm.KCL=0
              vm.KGL=KG.length
              vm.YGL=YG.length
              vm.PGL=PG.length
              vm.KCL=KC.length
            }


            vm.getGroupPoints()
            vm.getGroupSet=setInterval(function () {
              if(vm.isOpenGroup==1){
                vm.getGroupPoints()
              }
            },2000)

          },
          complete: function (XHR) {
            if(vm.getMapAct){
              clearInterval(vm.getMapAct);
              vm.getMapAct=null;
            }
            vm.getMapAct=setInterval(function () {
              vm.pointShow()
            },1000)
            XHR = null;  //回收资源

          }
        });

      },
      /*重刷基础点位*/
      pointRe:function () {
        let vm = this
//        vm.criminalLists.length=0
        let sendMessage  = {
          Header: {
            MsgID:"201501260000000035",
            MsgType:10
          },
          Body: JSON.stringify({
            MapID:vm.MapID,
            OrgID:localStorage.getItem("OrgID").toLowerCase(),
            ManageLevel:0,
            AreaID:"00000000-0000-0000-0000-000000000000",
            PSType:"2002"
          })
        }
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(sendMessage),
          success: function (result) {
            for (let i=0;i<result.length;i++){
              var ManageLevels=vm.personBand[0][result[i].PSID.toLowerCase()].ManageLevel
              var pointImg="";
              if(ManageLevels=="4201"){
                pointImg=vm.ipImg+"KG.png"
              }else if(ManageLevels=="4202"){
                pointImg=vm.ipImg+"PG.png"
              }else if(ManageLevels=="4203"){
                pointImg=vm.ipImg+"YG.png"
              }else if(ManageLevels=="4204"){
                pointImg=vm.ipImg+"KC.png"
              }

              vm.criminalLists.push({
                pointShows:true,
                PSID:result[i].PSID.toLowerCase(),
                Name:vm.personBand[0][result[i].PSID.toLowerCase()].Name,
                PersonID:vm.personBand[0][result[i].PSID.toLowerCase()].PersonID,
                OrgName:vm.personBand[0][result[i].PSID.toLowerCase()].OrgName,
                ManageLevelName:vm.personBand[0][result[i].PSID.toLowerCase()].ManageLevelName,
                CriminalPhoto:vm.personBand[0][result[i].PSID.toLowerCase()].Photo,
                status:false,
                ManageLevel:vm.personBand[0][result[i].PSID.toLowerCase()].ManageLevel,
                CriminalX:result[i].X,
                CriminalY:result[i].Y,
                pointImg:pointImg
              })
            }
            let KG=[]
            let YG=[]
            let PG=[]
            let KC=[]
            for (let j=0;j<vm.criminalLists.length;j++){
              if(vm.criminalLists[j].ManageLevel=="4201"){
                KG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4202"){
                PG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4203"){
                YG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4204"){
                KC.push(vm.criminalLists[j])
              }
              vm.KGL=0
              vm.YGL=0
              vm.PGL=0
              vm.KCL=0
              vm.KGL=KG.length
              vm.YGL=YG.length
              vm.PGL=PG.length
              vm.KCL=KC.length
            }
          },
          complete: function (XHR) {

            XHR = null;  //回收资源

          }
        });

      },
      /* 地图定位实时刷新 */
      pointShow:function () {
        let vm = this
        vm.getNewData()
//        vm.criminalLists=[]
        let sendMessage  = {
          Header: {
            MsgID:"201501260000000035",
            MsgType:10
          },
          Body: JSON.stringify({
            MapID:vm.MapID,
            OrgID:localStorage.getItem("OrgID").toLowerCase(),
            ManageLevel:0,
            AreaID:"00000000-0000-0000-0000-000000000000",
            PSType:"2002"
          })
        }
        $.ajax({
          type: "get",
          contentType: "application/json; charset=utf-8",
          dataType: "jsonp",
          jsonp: "callback",
          async: false,
          url: ajaxUrl,
          data:JSON.stringify(sendMessage),
          success: function (result) {
            var ps1=[]
            var ps2=[]
            if(result.length==0){
              vm.KGL=0
              vm.YGL=0
              vm.PGL=0
              vm.KCL=0
            }
            for(let m=0;m<result.length;m++){
              ps1.push(result[m].PSID.toLowerCase())
            }
            for(let n=0;n<vm.criminalLists.length;n++){
              ps2.push(vm.criminalLists[n].PSID.toLowerCase())
            }
            if(JSON.stringify(ps2.sort()) != JSON.stringify(ps1.sort())){
              vm.criminalLists.splice(0,vm.criminalLists.length);
              vm.pointRe()
            }
            for (let i=0;i<result.length;i++){
              for (let j=0;j<vm.criminalLists.length;j++){
                if(vm.criminalLists[j].PSID.toLowerCase()==result[i].PSID.toLowerCase()){
                  vm.criminalLists[j].Name=vm.personBand[0][result[i].PSID.toLowerCase()].Name
                  vm.criminalLists[j].PersonID=vm.personBand[0][result[i].PSID.toLowerCase()].PersonID
                  vm.criminalLists[j].OrgName=vm.personBand[0][result[i].PSID.toLowerCase()].OrgName
                  vm.criminalLists[j].ManageLevelName=vm.personBand[0][result[i].PSID.toLowerCase()].ManageLevelName
                  vm.criminalLists[j].CriminalPhoto=vm.personBand[0][result[i].PSID.toLowerCase()].Photo
                  vm.criminalLists[j].ManageLevel=vm.personBand[0][result[i].PSID.toLowerCase()].ManageLevel
                  vm.criminalLists[j].MapID=result[i].MapID
                  vm.criminalLists[j].CriminalX=result[i].X
                  vm.criminalLists[j].CriminalY=result[i].Y
                }
              }
            }
//            vm.criminalLists=  [
//              {Name:"1",
//                PersonID:1,
//                OrgName:"三监区",
//                status:false,
//                ManageLevelName:"宽管",
//                CriminalX:"400",
//                CriminalY:"500",
//                pointShows:true,
//                pointImg:"http://10.58.1.178:7706/dist/static/PG.png"},
//              {Name:"2",
//                OrgName:"三监区",
//                status:false,
//                ManageLevelName:"宽管",
//                PersonID:2,
//                CriminalX:"300",
//                CriminalY:"400",
//                pointShows:true,
//                pointImg:"http://10.58.1.178:7706/dist/static/PG.png"},
//              {Name:"3",
//                OrgName:"三监区",
//                status:false,
//                ManageLevelName:"宽管",
//                PersonID:3,
//                CriminalX:"300",
//                CriminalY:"400",
//                pointShows:true,
//                pointImg:"http://10.58.1.178:7706/dist/static/PG.png"},
//              {Name:"3",
//                OrgName:"三监区",
//                status:false,
//                ManageLevelName:"宽管",
//                PersonID:4,
//                CriminalX:"300",
//                CriminalY:"400",
//                pointShows:true,
//                pointImg:"http://10.58.1.178:7706/dist/static/PG.png"},
//              {Name:"3",
//                OrgName:"三监区",
//                status:false,
//
//                ManageLevelName:"宽管",
//                PersonID:5,
//
//                CriminalX:"700",
//                CriminalY:"400",
//                pointShows:true,
//                pointImg:"http://10.58.1.178:7706/dist/static/PG.png"},
//
//              {Name:"5",
//                OrgName:"三监区",
//                status:false,
//                ManageLevelName:"宽管",
//                PersonID:7,
//                CriminalX:"300",
//                CriminalY:"390",
//                pointShows:true,
//                pointImg:"http://10.58.1.178:7706/dist/static/PG.png"},
//              {Name:"6",
//                OrgName:"三监区",
//                status:false,
//                ManageLevelName:"宽管",
//                PersonID:8,
//                CriminalX:"320",
//                CriminalY:"400",
//                pointShows:true,
//                pointImg:"http://10.58.1.178:7706/dist/static/PG.png"},
//              {Name:"7",
//                OrgName:"三监区",
//                status:false,
//                ManageLevelName:"宽管",
//                PersonID:9,
//                CriminalX:"800",
//                CriminalY:"90",
//                pointShows:true,
//                pointImg:"http://10.58.1.178:7706/dist/static/PG.png"},
//              {Name:"8",
//                OrgName:"三监区",
//                status:false,
//                ManageLevelName:"宽管",
//                PersonID:89,
//                CriminalX:"800",
//                CriminalY:"90",
//                pointShows:true,
//                pointImg:"http://10.58.1.178:7706/dist/static/PG.png"},
//              {Name:"9",
//                OrgName:"三监区",
//                status:false,
//                ManageLevelName:"宽管",
//                PersonID:89,
//                CriminalX:"220",
//                CriminalY:"90",
//                pointShows:true,
//                pointImg:"http://10.58.1.178:7706/dist/static/PG.png"},]

            let KG=[]
            let YG=[]
            let PG=[]
            let KC=[]
            for (let j=0;j<vm.criminalLists.length;j++){
              if(vm.criminalLists[j].ManageLevel=="4201"){
                KG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4202"){
                PG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4203"){
                YG.push(vm.criminalLists[j])
              }else if(vm.criminalLists[j].ManageLevel=="4204"){
                KC.push(vm.criminalLists[j])
              }
              vm.KGL=0
              vm.YGL=0
              vm.PGL=0
              vm.KCL=0
              vm.KGL=KG.length
              vm.YGL=YG.length
              vm.PGL=PG.length
              vm.KCL=KC.length
            }

//            vm.getGroupPoints()

          },
          complete: function (XHR) {
            XHR = null;  //回收资源
          }
        });

      },
      /*返回*/
      cancle:function () {
        clearInterval(this.getMapAct)
        localStorage.setItem("criminalGroupIDs","")
        this.$router.push({ path: '/mutualsupervision' })
      },
      /*重合点位刷新*/
      getGroupPoints(){
        var vm=this
        var positionData=vm.criminalLists
//        console.log("positionData",positionData)
        var mergePointOffset=0
        var mergePointCollection = JSON.parse(JSON.stringify(positionData));
        for (var i = 0; i < mergePointCollection.length; i++) {
          var curDivPos = mergePointCollection[i];
          if (curDivPos.isTotal == undefined) {
            var divTotal = 1;
            var posMenu = [];
            for (var j = i; j < mergePointCollection.length; j++) {
              if (mergePointCollection[j].isTotal == undefined && mergePointCollection[j].CriminalX >= (curDivPos.CriminalX - mergePointOffset) && mergePointCollection[j].CriminalX <= (curDivPos.CriminalX + mergePointOffset)
                && mergePointCollection[j].CriminalY >= (curDivPos.CriminalY - mergePointOffset) && mergePointCollection[j].CriminalY <= (curDivPos.CriminalY + mergePointOffset)) {
                divTotal += 1;
                vm.criminalLists[j].pointShow=false

                mergePointCollection[j].isTotal = 1;
                var kk=[]
                for(var k=0;k<mergePointCollection.length;k++){
                  if(mergePointCollection[k].CriminalX ==mergePointCollection[j].CriminalX&&mergePointCollection[k].CriminalY ==mergePointCollection[j].CriminalY){
                    var shortData={
                      OrgName:mergePointCollection[k].OrgName,
                      PersonID:mergePointCollection[k].PersonID,
                      Name:mergePointCollection[k].Name,
                      ManageLevelName:mergePointCollection[k].ManageLevelName,
                      CriminalPhoto:mergePointCollection[k].CriminalPhoto,
                      CriminalX:mergePointCollection[k].CriminalX,
                      CriminalY:mergePointCollection[k].CriminalY,
                      PSID:mergePointCollection[k].PSID,
                      status:false,
                    }
                    kk.push(shortData)
                  }
                }
                posMenu.push(kk);

              }
            }
            if (divTotal > 2) {
              var groups=[]
              if(vm.criminalGroups!=null){
                for(let m=0;m<vm.criminalGroups.length;m++){
//                  groups.concat(vm.criminalGroups[m].posMenu[0])
                  for(let b=0;b<vm.criminalGroups[m].posMenu[0].length;b++){
                    groups.push(vm.criminalGroups[m].posMenu[0][b])
                  }

                }
              }


              for (let m=0;m<vm.criminalLists.length;m++){
                for (let b=0;b<groups.length;b++){
                  if(groups[b].PersonID==vm.criminalLists[m].PersonID){
                    vm.criminalLists[m].pointShows=false;
                    break;
                  }else {
                    vm.criminalLists[m].pointShows=true;
                  }
                }
//                vm.criminalLists[m].pointShows=true;
              }
              if (curDivPos.isTotal == undefined) {
                curDivPos = $.extend(curDivPos, { "isTotal": divTotal });
              }
              else {
                curDivPos.isTotal = divTotal;
              }
              if (curDivPos.posMenu == undefined) {
                curDivPos = $.extend(curDivPos, { "posMenu": posMenu });
              }
              else {
                curDivPos.posMenu = posMenu;
              }
            }
          }
        }
        var realData=[]
        for (let m=0;m<mergePointCollection.length;m++){
          if(mergePointCollection[m].posMenu!==undefined){
            realData.push(mergePointCollection[m])
          }
        }
        vm.criminalGroups=realData

      }
    },

    mounted () {
      let vm = this


//      vm.ipImg='http://'+window.location.host+'/dist/static/'
//      vm.ipImg='../../static/'
      vm.ipImg='http://10.58.1.178:7706/dist/static/'
      scaleNum=1
      $.ajax({
        type: "get",
        contentType: "application/json; charset=utf-8",
        dataType: "jsonp",
        jsonp: "callback",
        async: false,
        url: BasicUrl + 'HomeIndex/GetBindJQ',
        success: function (result) {
          vm.prisonSelect=result
          /*保存所有监区信息，人员分布渲染左侧地图用*/
          var allMapList = result;
          for (let i=0;i<allMapList.length;i++){
            allMapList[i].MapUrl=MapUrl+vm.mapList[0][allMapList[i].MapFlnkID].MapUrl
            if(i==0){
              allMapList[i].status=true
              vm.positionMap=allMapList[i].MapUrl
              vm.MapID=allMapList[i].MapFlnkID
            }else {
              allMapList[i].status=false
            }
          }
          vm.allMapLists=allMapList
          vm.prisonSelectText=vm.allMapLists[0].AreaName
//          console.log("allMapList",allMapList)
        },
        error: function (err) {
          console.log(err)
        }
      })
      /* Coding By YanM */
      setTimeout(function () {
        vm.pointS()
      },1500)
//      setTimeout(function () {
//        scaleNum=1020/$("#positionMap").width()
//        vm.changeSize("0")
//      },1000)
//     vm.getGroupSet=setInterval(function () {
//         if(vm.isOpenGroup==1){
//           vm.getGroupPoints()
//         }
//      },2000)
      $(".map").draggable();

      /*获取监区信息，用于左侧地图选择*/

      /* Coding By YanM */
    },
    destroyed: function () {
      clearInterval(this.getMapAct)
      clearInterval(this.getSearch)
      clearInterval(this.getGroupSet)
      localStorage.setItem("criminalGroupIDs","")
    }
  }
</script>

<style lang="scss" scoped>
  .personGroups{
    width: 36px;
    height: 35px;
    border-radius: 50px;
    color: white;
    background: red;
    line-height: 38px;
    position: absolute;
    top: 300px;
    left: 300px;
  }
  .personGroup{
    width: 141px;
    background: #2368e4;
    max-height: 166px;
  }
  .personName{
    width: 116px;
    color: white;
    font-size: 21px;
    text-align: center;
    line-height: 26px;
    margin: 0px auto;
    padding: 4px;
    border-bottom: 2px solid wheat

  }
  .personName:active{
    width: 116px;
    color: white;
    font-size: 21px;
    text-align: center;
    line-height: 26px;
    margin: 0px auto;
    background: red;
    border-bottom: 2px solid wheat

  }
  .searchInput::-webkit-input-placeholder{
    color: #fff;
  }
  .searchMode{
    position: absolute;
    z-index: 9999999;
    width: 1020px;
    background: #a2bcdb;
    height: 435px;
    border-radius: 7px;
    margin: 150px 338px;
  }
  .searchInput{
    width: 924px;
    height: 47px;
    margin: 18px 0px;
    font-size: 30px;
    text-align: center;
    background: #697a8f;
    color: white;
  }

  .searchBottons{
    width: 948px;
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

  .mapPic{
    margin: 10px auto;
    width: 218px;
    border: 2px solid rgba(0, 60, 242, 0);

  }
  .chosedMap{
    border: 2px solid #003cf2;
  }
  .li4_parts {
    height: 740px;
    margin: 0px auto;
  }
  .li4_parts .deailBody{
    background: #d9e3fe;
    height: 694px;
    overflow: auto;
  }

  .li4_parts .partsBody {
    width: 100%;
    height:780px;
    background: white;
  }
  .li4_parts .partsFoot{
    width: 100%;
    height: 84px;
    background: #c5cfdb;
  }
  .li4_parts .bodyCon{
    height: 592px;
  }
  .li4_parts .sure{
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
  .li4_parts .criminal{
    height: 150px;
    background: #0048d9;
    margin:10px 5px;
    padding: 5px;
  }
  .li4_parts .criminalName {
    font-size: 12px;
    color: white;
    display: inline-block;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    width: 99%;
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
  .criminal_active{
    background: #ce8900 !important;
  }
  .map{
    position: relative;
    display: inline-block;
    img{
      display: block;
    }
  }
  .point{
    /*width: 10px;*/
    /*height: 10px;*/
    /*background: red;*/
    position: absolute;
    /*border-radius: 18px;*/
    /*box-shadow: 1px 0px 10px 3px;*/
    /*animation:mymove 1s infinite;*/
    /*-webkit-animation:mymove 1s infinite; !*Safari and Chrome*!*/
  }
  @keyframes mymove
  {
    0% { box-shadow:1px 0px 10px 5px;}
    50% { box-shadow:1px 0px 10px 0px;}
    100%{ box-shadow:1px 0px 10px 5px;}
  }

  @-webkit-keyframes mymove /*Safari and Chrome*/
  {
    0% { box-shadow:1px 0px 10px 5px;}
    50% { box-shadow:1px 0px 10px 0px;}
    100%{ box-shadow:1px 0px 10px 5px;}
  }
  .pointed{
    /*background: blue;*/
  }
  .pointTop{
    width: 250px;
    color: white;
    background: #022171;
    text-align: center;
    height: 140px;
    padding: 3px;
    border-radius: 5px;
    margin: -189px -54px;
    position: absolute;
    z-index: 9999;
  }
  .slc{
    width: 34px;
    height: 34px;
    /*line-height: 32px;*/
    /*border-radius: 99px;*/
    /*font-size: 35px;*/
    /*border: 2px solid blue;*/
    margin: 14px 9px;
    float: left;
  }

  .personShow{
    float: left;
    margin: 8px 17px;
    border: 3px solid #5443ff;
    padding: 0px;
    height: 117px;
    width: 72px;
    -o-text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis
  }
  .personShowed{
    border: 3px solid #ff5718;
  }
  .perName{
    line-height: 30px;
  }
</style>
