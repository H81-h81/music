<!--  -->
<template>
  <div class="music-content">
    <div class="music-content-left">
      <div class="content-left">
        <div class="left-list" v-for="item in songList" :key="item.id">
          <div class="list-item-left" @click="getSong(item.id)"></div>
          <div class="list-item-mid">{{ item.name }}</div>
          <div
            @click="getMV(item.mvid)"
            :class="[
              'list-item-right',
              { hidden: item.mvid === 0 ? true : false },
            ]"
          ></div>
        </div>
      </div>
    </div>
    <div class="music-content-mid">
      <img
        src="/static/imgs/player_bar.png"
        alt=""
        :class="['player-bar', { playing: audioPlaying }]"
      />
      <img src="/static/imgs/111.jpg" alt="" class="cover" />
      <img
        src="/static/imgs/disc.png"
        alt=""
        :class="['disc', { autorotate: audioPlaying }]"
      />
    </div>
    <div class="music-content-right">
      <h1>热门留言</h1>
      <div class="comment-list">
        <div
          class="comment-list-item"
          v-for="comment in hotComments"
          :key="comment.commentId"
        >
          <div class="list-item-left">
            <img :src="comment.user.avatarUrl" alt="" />
          </div>
          <div class="comment-item-right">
            <div class="item-right-top">
              {{ comment.user.nickname }}
            </div>
            <div class="item-right-bottom">
              {{ comment.content }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
  //import引入的组件需要注入到对象中才能使用
  props: {
    songList: {
      type: Array,
      default: function () {
        return [];
      },
    },
    isAudioPlaying: {
      type: Boolean,
      default: true,
    },
  },
  components: {},
  data() {
    //这里存放数据
    return {
      audioPlaying: false,
      //热门评论
      hotComments: [],
    };
  },
  //监听属性 类似于data概念
  computed: {},
  //监控data中的数据变化
  watch: {
    isAudioPlaying: function (newValue) {
      this.audioPlaying = newValue;
    },
  },
  //方法集合
  methods: {
    getSong: async function (id) {
      //歌曲正在播放
      this.audioPlaying = true;
      // 第一个请求
      let res = await this.$http.get("song/url", {
        params: {
          id: id,
        },
      });
      let songUrl = res.data.data[0].url;
      this.$emit("geturl", songUrl);
      // 第二个请求
      //  获取热门评论
      let res2 = await this.$http.get("comment/hot", {
        params: {
          type: 0,
          id: id,
        },
      });
      this.hotComments = res2.data.hotComments;
    },
    getMV: async function (mvid) {
      let res = await this.$http.get("mv/url", {
        params: {
          id: mvid,
        },
      });
      let mvUrl = res.data.data.url;
      this.$emit("getmv", mvUrl);
      console.log(res);
    },
  },
  //生命周期 - 创建完成（可以访问当前this实例）
  created() {},
  //生命周期 - 挂载完成（可以访问DOM元素）
  mounted() {},
  beforeCreate() {}, //生命周期 - 创建之前
  beforeMount() {}, //生命周期 - 挂载之前
  beforeUpdate() {}, //生命周期 - 更新之前
  updated() {}, //生命周期 - 更新之后
  beforeDestroy() {}, //生命周期 - 销毁之前
  destroyed() {}, //生命周期 - 销毁完成
  activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
};
</script>
<style lang='less' scoped>
@keyframes running {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
.music-content {
  display: flex;
  .music-content-left {
    width: 200px;
    // border-right: 1px dotted red;
    height: 480px;
    .content-left {
      overflow-y: scroll;
      height: 480px;
      .left-list:nth-child(odd) {
        background-color: rgba(224, 217, 217, 0.5);
      }
      .left-list {
        display: flex;
        align-items: center;
        justify-content: space-between;
        height: 50px;
        .list-item-left {
          width: 25px;
          height: 25px;
          background: url(/static/imgs/table.png) -18px -18px;
        }

        .list-item-mid {
          overflow: hidden;
          text-overflow: ellipsis;
          height: 30px;
          line-height: 30px;
          color: white;
          width: 150px;
          text-align: center;
        }
        .hidden {
          visibility: hidden;
        }
        .list-item-right {
          width: 30px;
          height: 20px;
          background: url(/static/imgs/table.png) left -45px;
        }
      }
    }
  }

  .music-content-mid {
    width: 400px;
    // border-right: 1px dotted green;
    height: 480px;
    position: relative;
    .cover {
      top: 156px;
      left: 156px;
      position: absolute;
      width: 150px;
      height: 150px;
    }
    .autorotate {
      animation-name: running;
      animation-duration: 5s;
      animation-timing-function: linear;
      animation-iteration-count: infinite;
    }
    .disc {
      position: absolute;
      left: 100px;
      top: 100px;
    }
    .playing {
      transform: rotate(0) !important;
      
    }
    .player-bar {
      left: 200px;
      position: absolute;
      z-index: 8;
      transform: rotate(-25deg);
      transform-origin: 12px 12px;
      transition: all 2s;
    }
  }

  .music-content-right {
    width: 200px;
    height: 480px;
    h1 {
      font-weight: 400;
      font-size: 16px;
      padding: 5px;
    }

    .comment-list {
      overflow-y: scroll;
      height: 460px;
      .comment-list-item {
        display: flex;
        .list-item-left {
          img {
            width: 30px;
            height: 30px;
            border-radius: 50px;
          }
        }

        .comment-item-right {
          .item-right-top {
            font-size: 16px;
          }

          .item-right-bottom {
            font-size: 12px;
            color: white;
          }
        }
      }
    }
  }
}
</style>