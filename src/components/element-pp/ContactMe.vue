<template>
  <div class="cm-container">
    <div class="cm-left"></div>
    <div class="cm-middle">
      <el-steps :active="curActive">
        <el-step title="phone number" @click="changeActive(0)">
          <template v-slot:icon>
            <img class="smartphone-icon" src="~assets/index/images/contact/smartphone.png" alt="phone">
          </template>
          <template v-slot:description>
            <a class="my-telephone" href="tel:0668252048">+33 6 68 25 20 48</a>
          </template>
        </el-step>
        <el-step title="e-mail" @click="changeActive(1)">
          <template v-slot:icon>
            <img class="email-icon" src="~assets/index/images/contact/email.png" alt="email">
          </template>
          <template v-slot:description>
            <a class="my-email" target="_blank" href="mailto:yuan.cao@utbm.fr">yuan.cao@utbm.fr</a>
          </template>
        </el-step>
        <el-step title="linkedin" @click="changeActive(2)">
          <template v-slot:icon>
            <img class="linkedin-icon" src="~assets/index/images/contact/linkedin.png" alt="linkedin">
          </template>
          <template v-slot:description>
            <a class="my-linkedin" target="_blank" href="https://www.linkedin.com/in/yuan-cao-75b112208">linkedin.com/in/yuan-cao-75b112208</a>
          </template>
        </el-step>
        <el-step title="location" @click="changeActive(3)">
          <template v-slot:icon>
            <img class="location-icon" src="~assets/index/images/contact/location.png" alt="location">
          </template>
          <template v-slot:description>
            <span class="my-direction"  @click="mapDrawer=true">Belfort, France</span>
            <span class="locate-me">
              <img class="locate-img" src="~assets/index/images/contact/locate.png" alt="locate me">
            </span>
          </template>
        </el-step>
      </el-steps>
    </div>
    <div class="cm-right"></div>
  </div>
  <el-dialog v-model="mapDrawer" @open="openedEve"
             close-on-press-escape width="35%" top="12vh" center>
    <template v-slot:title>
      <span style="color: blue">LOCATE ME ON GOOGLE MAP</span>&nbsp;
      <img class="locate-me-icon" src="~assets/index/images/contact/locateOnMap.png" alt="location">
    </template>
    <div class="dialog-content" style="height: 400px">
      <div class="google-map" v-loading="ifMapLoaded" style="height: 100%">
        <google-map-a-p-i></google-map-a-p-i>
      </div>
    </div>

  </el-dialog>
</template>

<script>
import {markRaw} from "@vue/reactivity";
import {LocationInformation, Message, Phone} from "@element-plus/icons-vue";
import GoogleMapAPI from "../common/GoogleMapAPI";

export default {
  name: "ContactMe",
  data(){
    return {
      //检测gmap是否加载完毕
      ifMapLoaded:true,
      monitorTimer:null,
      mapDrawer:false,
      Message: markRaw(Message),
      Phone: markRaw(Phone),
      LocationInformation: markRaw(LocationInformation),
      curActive:4,
    }
  },
  methods:{
    openedEve(){
      //每隔200ms判断是不是已经加载完毕（已经生成了html），以此起到监视器的效果
      this.monitorTimer =  setInterval(function (){
        if(document.getElementsByClassName('vue-map').length>0){
          this.ifMapLoaded=false
          clearInterval(this.monitorTimer)
        }
      }.bind(this),200)
    },
    changeActive(ind){
      this.curActive=this.curActive===ind?4:ind;
    },
    //删除GoogleMapApi组件里生成的所有script标签
    deleteScript:(done)=>{
      document.getElementById('google-map-api').remove()
      done();
    }
  },
  computed:{
    getIcon1(){
      return require("@/assets/index/images/contact/linkedin.png")
    }
  },
  components:{
    GoogleMapAPI
  },
  mounted() {
    // window.addEventListener('load',()=>{
    //   this.ifMapLoaded=false;
    // })
    // window.addEventListener('unload',()=>{
    //   this.ifMapLoaded=true;
    // })
  }
}
</script>

<style scoped>
.cm-container{
  display: flex;
  height: 150px;
  width: 100%;
  /*div固定在底部*/
  position: fixed;
  bottom: 0;
  z-index: 2;
  opacity: 0.9;
}
.cm-left,.cm-right{
  flex: 1;
}
.cm-middle{
  flex: 2;
  padding: 20px;
  margin-top: 20px;
  background-color: #e8e9af;
;
}
a{
  color: #0022ff;
}
.my-direction{
  text-decoration: underline;
  color: blue;
  cursor: pointer;
}
.locate-me{
  margin-left: 3px;
  position: relative;
  top: 3px
}
.locate-img{
  width: 14px;
}
.locate-me-icon{
  width: 18px;
  position: relative;
  top: 1px;
}
/*a:link{*/
/*  color: #0022ff*/

/*}*/
/*a:visited{*/
/*  color: #5469f1*/
/*}*/
</style>