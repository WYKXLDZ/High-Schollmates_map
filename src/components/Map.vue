<template>
  <el-container :class="'main'+(darkmode?' dark_mode':'')" v-loading="loading">
    <div id="show">
      <div id="start_show" style="top: 0%;">
<!--                 <img src="./bg.svg" id="buffer">
-->            </div>
      <div id="main_show">
              <div class="head" @click="center_to({coordinate:location})">  
                  <div class="title">
                              Where Are You, My Classmate ? 
                  </div>
              </div>
              <div id="cheak_borad">
                  <div id="name">
                      <el-input class="input_left" id="input_name" v-model="searched_name" placeholder="Input the Name"></el-input>
                      <div onclick="search()" id="check_name">Search</div>
                  </div>
                  <!-- <div id="tools">
                    <div class="btn" @click="info_show=true;add_new_show=true;">
                      <i class="el-icon-plus"/>
                    </div>
                  </div> -->
                  <div id="container">
                  </div>
              </div>
              <div class="footer">
                      <div class="B" style="height:50px">
                          <div class="num_b" params="girls" @click="info_show=true;info.g.show=true;">
                              <div class="img_"></div>
                              <div class="num">{{info.g.list.length}}</div>
                          </div>
                          <div class="num_b" params ="boys">
                              <div class="img_" style="background-image:url(../assets/boy.svg)"></div>
                              <div class="num">{{info.b.list.length}}</div>
                          </div>
                          <div class="num_b" params="teachers">
                              <div class="img_" style="background-image:url(../assets/teacher.svg)"></div>
                              <div class="num">{{info.t.list.length}}</div>
                          </div>
                      </div>
              </div>
      </div>
      <div id="info" :class="(info_show?'show':'')">
      <div style="position:absolute;top:0px;left:0px;height:100%;width:100%;" @click="close_info"></div>
      <div class="board" v-if="info.g.show" :style="{minHeight:'calc(50%)',transform:info_2_show?'scale(0.9) translate(0px, 5%)':''}">
        <div>
          <div class="item" v-for="item in info.g.list" :key="item.name" >
             <div style="width: calc(100% - 50px);">
              <div class="title wrap_style">{{item.name}}</div>
              <div class="content">{{item.university}}</div>
            </div>
            <div style="width:50px">
              <div class="btn" @click="open_info2();classmate_info.show=true;classmate_info.info=item;"><i class="el-icon-document"/></div>
            </div>
          </div>
        </div>
        <div  class="item">
          <div style="height:45px;line-height:45px;font-weight:bold;">添加人物</div>
          <div class="btn" @click="open_info2();add_new_show=true;">
            <i class="el-icon-plus"/>
          </div>
        </div>
      </div>

      <!-- <div class="board" v-if="add_new_show" :style="{minHeight:'calc(50%)',transform:info_2_show?'scale(0.9) translate(0px, 5%)':''}">
        <div id="city_name" style="border-radius: 10px;overflow: hidden;height: 45px;box-shadow: 0px 2px 10px rgba(0,0,0,.2);margin-bottom:10px;">
            <el-input class="input_left" id="input_name" v-model="searched_city_name" placeholder="Input the University"></el-input>
            <div @click="search_place(searched_city_name)" id="check_name">Search</div>
        </div>
        <div class="item" v-for="item in pois" :key="item.name">
            <div style="width: calc(100% - 50px);">
              <div class="title wrap_style">{{item.name}}</div>
              <div class="content" style="margin-top: -10px;">{{item.address}} <span v-if="item.tel!=''">｜ <i class="el-icon-phone"/>{{item.tel}}</span></div>
              <div class="content">{{item.type}}</div>
            </div>
            <div style="width:50px">
              <div class="btn" @click="open_info2();"><i class="el-icon-plus"/></div>
            </div>
        </div>

      </div> -->
      <div :class="'board second'+(info_2_show?' show':'')">
        <div style="height:50px;">
          <div class="btn" @click="info_2_show=false"><i class="el-icon-arrow-left"/></div>
        </div>
        <div class="classmate_info" v-show="classmate_info.show">
          <div class="title">{{classmate_info.info.name}}</div>
        </div>
        <div  v-if="add_new_show" >
          <div style="margin:10px 0px;">
            <el-input class="input_left" id="input_name" v-model="add_new.classmate" placeholder="Input the Name"></el-input>
            <div class="item" v-for="item in add_new.university_list" :key="item.name">
                <div style="width: calc(100% - 50px);">
                  <div class="title wrap_style">{{item.name}}</div>
                  <div class="content" style="margin-top: -10px;">{{item.address}} <span v-if="item.tel!=''">｜ <i class="el-icon-phone"/>{{item.tel}}</span></div>
                  <div class="content">{{item.type}}</div>
                </div>
                <div style="width:50px">
                  <div class="btn red" @click="rm_from_new(item);"><i class="el-icon-minus"/></div>
                </div>
            </div>
            <div class="line"></div>
          </div>
          <div id="city_name" style="border-radius: 10px;overflow: hidden;height: 45px;box-shadow: 0px 2px 10px rgba(0,0,0,.2);margin-bottom:10px;">
              <el-input class="input_left" id="input_name" v-model="searched_city_name" placeholder="Input the University"></el-input>
              <div @click="search_place(searched_city_name)" id="check_name">Search</div>
          </div>
          <div class="item" v-for="item in pois" :key="item.name" v-show="item.show">
              <div style="width: calc(100% - 50px);">
                <div class="title wrap_style">{{item.name}}</div>
                <div class="content" style="margin-top: -10px;">{{item.address}} <span v-if="item.tel!=''">｜ <i class="el-icon-phone"/>{{item.tel}}</span></div>
                <div class="content">{{item.type}}</div>
              </div>
              <div style="width:50px">
                <div class="btn" @click="add_to_new(item);"><i class="el-icon-plus"/></div>
              </div>
          </div>

        </div>
      </div>
    </div>
    </div>
    
  </el-container>
</template>

<script>
// const $ = require("jquery");

// import gitee_info from '../index';
// import {GiteeAPI} from "../api";
// const gAPI = new GiteeAPI(gitee_info);

import AMapLoader from '@amap/amap-jsapi-loader';

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data:function(){
    return {
      darkmode:false,
      loading:false,
      amap_key:"4c8a518058fea0730e009e9ea3773473",
      padding:10,
      searched_name:"",
      location:null,
      info_show:false,
      info_2_show:false,
      info:{
        g:{
          show:false,
          list:[{
            name:"xx",
            coordinate: [104.088119, 30.637545],
            university :'四川大学',
          }]
        },
        b:{
          show:false,
          list:[]
        },
        t:{
          show:false,
          list:[]
        },
      },
      classmate_info:{show:false,info:{}},
      searched_city_name:"",
      pois:[],

      add_new_show:false,
      add_new:{
        classmate:"",
        university_list:[]
      }
    }
  },
  mounted:function(){
    setTimeout(function(){
        document.getElementById("start_show").style.top = "120%";
        document.getElementById("start_show").style.opacity = ".5";
    },1500);
    AMapLoader.load({
        "key": this.amap_key,              // 申请好的Web端开发者Key，首次调用 load 时必填
        "version": "1.4.15",   // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
        "plugins": ["AMap.PlaceSearch"],           // 需要使用的的插件列表，如比例尺'AMap.Scale'等
        // "AMapUI": {             // 是否加载 AMapUI，缺省不加载
        //     "version": '1.1',   // AMapUI 缺省 1.1
        //     "plugins":[],       // 需要加载的 AMapUI ui插件
        // },
        // "Loca":{                // 是否加载 Loca， 缺省不加载
        //     "version": '1.3.2'  // Loca 版本，缺省 1.3.2
        // },
    }).then((AMap)=>{
        this.map = new AMap.Map('container',{
          mapStyle: 'amap://styles/macaron',
          zoom: 17
        });
        this.placeSearch = new AMap.PlaceSearch(
          {
            city: '全国'
          });
        this.location = this.map.getCenter();
    }).catch(e => {
        console.log(e);
    })
  },
  methods:{
    add_to_new:function(item){
      let d = JSON.parse(JSON.stringify(item));
      item.show = false;
      this.add_new.university_list.push(d);
    },
    rm_from_new:function(item){
      let idx = 0;
      this.add_new.university_list.forEach(ele=>{
        if(item.id == ele.id){
          this.add_new.university_list.splice(idx,1);
          this.$message({
            message: '移除成功',
            type: 'warning',
            customClass:"message",
          });
          return false;
        }
        idx+=1;
      })
      this.pois.forEach(ele=>{
        if(item.id == ele.id){
          ele.show=true;
        }
      })
    },
    open_info2:function(){
      this.info_2_show=true;
      this.classmate_info.show=false;
      this.add_new_show=false;
    },    
    close_info:function(){
      this.info_show=false;
      this.info_2_show=false;
      this.info.g.show=false;
      
    },
    center_to:function(item){
      this.map.setCenter(item.coordinate);
      this.info_show=false;
    },
    search_place:function(keywords){
      this.loading=true;
      this.placeSearch.search(keywords, (status, result)=>{
        this.pois.splice(0,this.pois.length)
        if(status!="complete"){
          this.$message({
            message: '获取列表为空',
            type: 'warning',
            customClass:"message",
          });
          this.loading=false;
          return;
        }
        this.pois = result.poiList.pois;
        if(this.pois.length==0)this.$message({
          message: '获取列表为空',
          type: 'warning',
          customClass:"message",
        });
        else this.$message({
          message: "搜索成功",
          type: 'success',
          customClass:"message",
        })
        this.pois.forEach(ele=>{ele.show=true;});
        this.loading=false;
        console.log(status,result)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.message{
    border-radius: 10px;
    background: white;
    border: 0px;
    box-shadow: 0px 3px 10px rgba(0,0,0,.2);
    min-width: auto;
}
</style>
<style lang="scss" rel="stylesheet/scss" scoped>
.wrap_style{
  overflow: hidden;
text-overflow:ellipsis;
white-space: nowrap;
}
.main{
  position:fixed;
  top:0px;
  left: 0px;
  height:100%;
  width:100%;
}

.dark_mode{
  background:#333;
}
.page-content{background:-webkit-linear-gradient(#2F5A53,#6CA199); top:352px; min-height:0;_height:0px;width:0px; }
*{margin: 0px;padding: 0px;}

body{
background: #405eca;
font-weight:100;
}

#show{
      height: calc(100% - 20px);
    width: calc(50% - 20px);
    max-width: 767px;
    position: relative;
    // right:50%;
    margin: auto;
    border-radius: 10px;
    box-shadow: 0px 5px 20px rgba(0,0,0,.2);
    overflow: hidden;
}
#info{
    height: 100%;
    width: 100%;
    background: rgba(0,0,0,.4);

    -webkit-backdrop-filter: saturate(180%) blur(20px);
    backdrop-filter: saturate(180%) blur(20px);
    position: absolute;
    top: 0px;
    left: 0px;
    overflow: hidden;
    transition:ease .3s;
    z-index: -2;
    opacity: 0;
}
#info.show{
  z-index: 1;
  opacity: 1;
}
#show #start_show{
transition:ease .5s;
background:url("../assets/bg.jpeg");
background-color:white; 
position: fixed;
top: 0;
left: 0;
height: 100%;
width: 100%;
background-position: center 0;
background-repeat: no-repeat;
background-size: cover;
-webkit-background-size: cover;
-o-background-size: cover;
display: block;
z-index: 2;
float:left;
color:white;}
#buffer{width:120px;height:75px;margin:10px;}
#show #main_show{height:100%;width:100%;display: block;float:right;}
.head{
height: 50px;
width: 100%;
}
#icon{
        width: 40px;
height: 30px;
float: left;
margin: 10px calc(5% + 5px);
margin-right: 0px;
}
.title{
    width: calc(100% - 20px);
    margin: 10px;
    height: 50px;
    line-height: 50px;
    text-align: center;
    font-size: 16px;
    font-family: -webkit-body;
    color: #444;
    background-color: #d1c4e9;
    border-radius: 10px;
    font-weight: bold;
    box-shadow: 0px 3px 10px 0px #d1c4e9;
}
#cheak_borad{}
#name{
background: white;
height: 45px;
width: calc(100% - 20px);
margin: 10px;
box-shadow: 0px 3px 10px -7px;
border-radius: 10px;
overflow: hidden;
}

#check_name{    background: #d1c4e9;
    height: 45px;
    color: white;
    width: 30%;
    border: none;
    border-radius: 0px;
    float: right;
    line-height: 45px;
    font-size: 18px;
    font-weight: bold;}
#container{height:100%;width:100%;position:absolute;top:0;left:0;z-index:-1;}
.footer{
/*                    height: 50px;
*/                    width: 100%;
    position: absolute;
    background: white;
    bottom: 0px;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
    box-shadow: 0px 0px 10px -6px;
    padding-top: 15px;
}
.footer:before{
content:"";
    height: 5px;
width: 50px;
background: #d0d0d0;
position: absolute;
border-radius: 10px;
top: 5px;
left: calc((100% - 50px)/2);
}
.B{
width:100%;
display: flex;
justify-content:space-around;
}
.num_b{
transition:ease .3s;
    display: flex;
height: 30px;
justify-content: center;
background: #EEE;
padding: 0px 20px;
border-radius: 30px;
margin-top: 10px;
}
.num_b .img_{
      height: 30px;
width: 30px;
background-size: 20px;
background-position: center;
background-repeat: no-repeat;
background-image: url(../assets/girl.svg);
}
.num_b .num{
        font-size: 14px;
line-height: 30px;
margin: 0px 10px;
}
.list_b{
transition:ease .5s;
    max-height: 0px;
width: 90%;
border-top: 1px solid #EEE;
overflow: scroll;
display: flex;
justify-content: space-between;
flex-wrap: wrap;
padding: 0px 5%;
}
.list_b .name_handle{
        height: 30px;
    padding: 0px 25px;
    border-radius: 30px;
    background: #EEE;
    margin: 10px 5px;
    text-align: center;
    font-size: 16px;
    line-height: 30px;
    color: #4a4a4a;
    transition:ease .5s;
}
.el-input.input_left{
height: 45px;
    width: 70%;
  ::v-deep #input_name{    
    background: white;
    
    border: none;
    border-color: #d1c4e9;
    text-align: center;
    float: left;
    color: #444;
    font-size: 20px;
    }
}

.board{
      height: auto;
    // min-height: calc(50%);
    max-height: calc(90% - 50px);
    position: absolute;
    bottom: 10px;
    width: calc(100% - 20px);
    background: white;
    left: 10px;
    border-radius: 10px;
    padding: 16px 10px;
    box-sizing: border-box;
    overflow: auto;
    transition: ease .3s;
}
.board.second{
  bottom:30px;
  height:calc(100% - 80px);
  box-shadow: 0px 3px 10px rgba(0,0,0,.2);
  z-index: -1;
  opacity: 0;
  transform: scale(.8);
  transition: ease .3s;
}
.board.second.show{
  z-index: 1;
  opacity: 1;
  transform: scale(1);
}
.board .item{
      background: rgba(0,0,0,.05);
    padding: 10px;
    box-sizing: border-box;
    border-radius: 10px;
    margin: 5px 0px;
    display: flex;
    justify-content: space-between;
    .title{
      background: transparent;
    box-shadow: 0px 0px 0px;
    padding: 5px;
    height: 30px;
    line-height: 30px;
    text-align: justify;
    margin: 0px;
    font-size: 1.2em;
    font-weight: bold;
    }
    .content{
      text-align: justify;
    padding: 5px 10px;
    font-size: .8em;
    }
}
.btn{
    height: 45px;
    width: 45px;
    background: #d1c4e9;
    border-radius: 45px;
    box-shadow: 0px 3px 10px 0px #d1c4e9;
    color: white;
    font-weight: bold;
    font-size: 1.2em;
    line-height: 45px;
  }
  .btn.red{
    background: #ef9a9a;
    box-shadow: 0px 3px 10px 0px #ef9a9a;
  }
#tools{
  width: calc(100% - 20px);
  margin:10px;
  .btn{
    margin: 5px;
    float: right;
  }
}
.line{
  width:100%;
  border-bottom:1px solid #eee;
}
.classmate_info{
  .title{
      background: transparent;
    box-shadow: 0px 0px 0px;
    padding: 5px;
    height: 30px;
    line-height: 30px;
    text-align: center;
    margin: 10px 0px;
    font-size: 1.4em;
    font-weight: bold;
    }
}
@media only screen and (max-width: 767px){
  #show{
    margin: 0px;
    border-radius: 0px;
    right:0px;
    width: 100%;
    height: 100%;
  }
}
</style>
