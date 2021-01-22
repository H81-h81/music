<template>
  <div id="app">
    <div class="app-player">
      <!-- 音乐盒的开始 -->
    <div class="app-player-header">
      <my-music-header @getlist="handleGetMusicList"></my-music-header>
    </div>
    <!-- 音乐盒中间内容 -->
    <div class="app-player-content">
      <my-music-content
      :is-audio-playing="isAudioPlaying"
       :song-list="songList"
        @geturl="handleGetSongUrl"
        @getmv="handleGetMVUrl"
        ></my-music-content>
    </div>
    <!-- 音乐盒底部 -->
    <div class="app-player-footer ">
      <my-music-footer
       :song-url="songUrl" 
       @pause="handlePause" 
       @play="handlePlay"
       ></my-music-footer>
    </div>
    </div>
<div :class="['app-mask-centent',{hidden:hiddenVedio}]">
      <div class="app-player-close" @click="handleCloseMV">x</div>
    <div class="app-player-vedio">
      <video
      ref="vedio"
       :src="mvUrl"
       controls="controls"
      loop="loop"
      autoplay="autoplay"
       ></video>
    </div>
    <div class="app-mask"></div>
</div>
  </div>
</template>

<script>
// import axios from 'axios'
// window.axios = axios;
import MyMusicHeader from "./components/my-music-header"
import MyMusicContent from "./components/my-music-content"
import MymusicFooter from "./components/my-music-footer"
export default {
  name: 'App',
data:function(){
  return {
    songList:[],
    songUrl:"",//歌曲的url
    mvUrl:"",
    hiddenVedio:true,  //默认情况下不显示 vedio
    isAudioPlaying:true
  };
},
  components: {
  "my-music-header":MyMusicHeader,
  "my-music-content":MyMusicContent,
  "my-music-footer":MymusicFooter,
  },
  methods:{
    handlePlay:function(isAudioPlaying){
      this.isAudioPlaying = isAudioPlaying;
    },
    handlePause:function(isAudioPlaying){
      this.isAudioPlaying = isAudioPlaying;
    },
    handleCloseMV:function(){
      this.hiddenVedio = true;
      // 关闭视频
      this.$refs.vedio.pause();
    },
    handleGetMusicList:function(songList){
      this.songList = songList;
    },
    handleGetSongUrl:function(songUrl){
      this.songUrl =songUrl;
    },
    handleGetMVUrl:function(mvUrl){
      this.mvUrl = mvUrl;
      // 显示弹出窗
      this.hiddenVedio = false;
    }
  },
};
</script>

<style>
#app {
  background: url(../public/static/imgs/456.jpeg) no-repeat;
  width: 100vw;
  height: 100vh;
  background-size: 100% 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  --themeColor:#1baad5;
}
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.app-player{
  width: 800px;
  height: 600px;
  border-radius: 10px;
}
.app-player-header {
 height: 60px;
  width: 100%;
  background-color: var(--themeColor);
}

.app-player-content {
height: 480px;
  width: 100%;
  display: flex;
  border-bottom: 1px dotted var(--themeColor);
  background-color: rgba(255, 255, 255, 0.5);
}

.app-player-footer {
 height: 60px;
  width: 100%;
  background-color: var(--themeColor);
}
.app-mask{
  height: 100vh;
  width: 100vw;
  background-color: black;
  position: fixed;
  top: 0;
  left: 0;
}
.app-player-vedio{
  transform: translate(-50%,-50%);
  width: 600px;
  height: 800px;
  position: fixed;
  top: 50%;
  left: 50%;
  z-index: 999;
}
video{
  width: 600px;
  height: 800px;
}
.app-player-close{
  position: fixed;
  color: white;
  top: 0;
  right: 10px;
  z-index: 1000;
  font-size: 30px;
    cursor: pointer;
}
.app-mask-centent{
  
}
.hidden{
  display: none;
}
</style>
