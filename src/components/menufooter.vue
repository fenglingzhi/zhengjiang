<template>
  <div class="menufooter">
    <el-row class="menu_title_wrap">
      <el-col :span="2" class="menu_title"></el-col>
      <el-col :span="5" class="menu_title" v-for="item in menuList" :key="1">
        <div @click="gopage(item.path)">{{item.name}}</div>
      </el-col>
      <el-col :span="2" class="menu_title"></el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: 'menufooter',
  props:['YHDL','canRouter'],
  data () {
    return {
      menuList: [
        {name: '监区概况', path: '/'},
//        {name: '出工收工', path: '/outwork'},
        {name: '人员清点', path: '/crimalcheck'},
        {name: '人员分布', path: '/positionShow'},
//        {name: '工具清点', path: '/toolcheck'},
        {name: '流动登记', path: '/outregister'},
//        {name: '互监组管理', path: '/mutualsupervision'},
//        {name: '人员绑卡', path: '/cardbind'},
//        {name: '人员解绑', path: '/cardUnBind'}

      ],
    }
  },
  methods: {

    gopage: function (path) {
      var vm = this
      // 路由跳转
//      this.$router.push({ path: path })
//      if(path === '/outwork' || path === '/outregister'){
//        vm.$emit('openLogin',true)
//      }
//
      if(path === '/outwork' ){
        if(localStorage.getItem("moveTypes")==2){
//          vm.$emit('routerTip',"请先完成临时外出登记")
          vm.$message("请先完成临时外出登记")

        }else if(localStorage.getItem("moveTypes")==3){
          vm.$message("请先完成临时外出登记")
        }else {
          this.$router.push({ path: '/outwork' })
          vm.$emit('openLogin',true)
        }
      }else if(path === '/outregister'){
        if(localStorage.getItem("moveTypes")==1){
//          vm.$emit('routerTip',"请先完成进出工登记")
          vm.$message("请先完成进出工登记")

        }else if(localStorage.getItem("moveTypes")==3){
//          vm.$emit('routerTip',"请先完成人员绑卡操作")
          vm.$message("请先完成人员绑卡操作")

        }else if(localStorage.getItem("moveTypes")==4){
//          vm.$emit('routerTip',"请先完成人员解绑操作")
          vm.$message("请先完成人员解绑操作")

        }else {
          this.$router.push({ path: '/outregister' })
          vm.$emit('openLogin',true)
        }

      }else if(path === '/positionShow'){
        this.$router.push({ path: '/positionShow' })
      }else if(path === '/crimalcheck'){
          this.$router.push({ path: '/crimalcheck' })
      }else if(path === '/toolcheck'){
          this.$router.push({ path: '/toolcheck' })
      }else if(path === '/mutualsupervision'){
          this.$router.push({ path: '/mutualsupervision' })
      }else if(path === '/') {
          this.$router.push({ path: '/' })
      }else if(path === '/cardbind') {
        if(localStorage.getItem("moveTypes")==2){
//          vm.$emit('routerTip',"请先完成外出登记")
          vm.$message("请先完成外出登记")

        }else if(localStorage.getItem("moveTypes")==1){
//          vm.$emit('routerTip',"请先完成进出工登记")
          vm.$message("请先完成进出工登记")

        }else {
          this.$router.push({ path: '/cardbind' })
          vm.$emit('openLogin',true)
        }
      }else if(path === '/cardUnBind') {
        if(localStorage.getItem("moveTypes")==2){
//          vm.$emit('routerTip',"请先完成外出登记")
          vm.$message("请先完成外出登记")

        }else if(localStorage.getItem("moveTypes")==1){
//          vm.$emit('routerTip',"请先完成进出工登记")
          vm.$message("请先完成进出工登记")

        }else {
          this.$router.push({ path: '/cardUnBind' })
          vm.$emit('openLogin',true)
        }
      }
    },
  }
}
</script>

<style lang="scss">
a{
  color: #fff;
  text-decoration: none;
}
.menufooter{
  padding:0 25px;
  height: 122px;
  color: #fff;
  overflow: auto;
  .menu_title_wrap{
    height:70px;
    border-bottom: 1px solid rgba(255, 255, 255, 0.25);
    line-height: 70px;
    margin: 20px 30px 0 30px;
  }
  .menu_title{
    height:70px;
    cursor: pointer;
    font-size: 26px;
  }
  .menu_title:hover{
    border-bottom: 3px solid #fff;
  }
}
</style>
