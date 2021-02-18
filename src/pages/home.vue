<template>
  <div class="barrage_area">
    <!-- 弹幕 -->
    <div
      class="dmGroup"
      v-for="(item, index) in dmData"
      :key="index"
      v-bind:style="{
        top: item.top + '%',
        animationName: 'dmAnimation',
        animationDuration: item.time + 's',
        animationTimingFunction: 'linear',
        animationDelay: index * 3 + 's',
        animationIterationCount: 'infinite',
        animationPlayState: 'running'
      }"
    >
      <div class="dmItem">
        <div class="dm">
          <div class="avatarBox">
            <img :src="item.avatar" class="avatar" />
          </div>
          <span class="content"
            >{{ item.nickname + "：" }}{{ item.content }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      dmData: []
    };
  },
  mounted() {
    this.getDataList();
  },
  methods: {
    getDataList() {
      axios
        .get("http://musicapi.leanapp.cn/comment/playlist?id=1")
        .then(res => {
          if (res.data.code === 200) {
            this.dmData = res.data.comments;
            this.parameters(this.dmData);
          }
        });
    },
    //处理弹幕参数
    parameters(e) {
      const dmArr = [];
      const _b = e;
      for (let i = 0; i < _b.length; i++) {
        const time = Math.floor(Math.random() * 10);
        const second = Math.floor(Math.random() * 60);
        const _time = time < 6 ? 6 + i : time + i;
        const top = Math.floor(Math.random() * 80) + 2;
        const _p = {
          id: _b[i].id,
          top,
          second,
          time: _time,
          content: _b[i].content,
          nickname: _b[i].user.nickname,
          avatar: _b[i].user.avatarUrl
        };
        dmArr.push(_p);
      }
      this.dmData = dmArr;
    }
  }
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
/* // 弹幕 */
.dmGroup {
  position: absolute;
  top: 70px;
  left: 100%;
  z-index: 10;
  animation-timing-function: linear;
  animation-fill-mode: none;
  transform: translateZ(0);
  white-space: nowrap;
  height: 30px;
  animation: dmAnimation 5s linear 2s infinite alternate;
}

.dmItem {
  display: inline-flex;
  margin-right: 30px;
  white-space: nowrap;
}

.dmItem .dm {
  display: inline-flex;
  vertical-align: middle;
  align-items: center;
  position: relative;
  height: 25px;
  line-height: 25px;
  padding: 0 7.5px 0 2.5px;
  background: rgba(0, 2, 2, 0.28);
  border-radius: 12.5px;
  overflow: hidden;
  font-size: 12px;
  color: rgba(255, 255, 255, 1);
}

.dmItem .avatarBox {
  display: inline-block;
  position: relative;
  width: 20px;
  height: 20px;
  margin-right: 5px;
}

.dmItem .avatarBox .avatar {
  width: 20px;
  height: 20px;
  border: 0;
  border-radius: 50%;
  overflow: hidden;
}

.dmItem .dm .content {
  display: inline-block;
  max-width: 250px;
  height: 25px;
  line-height: 25px;
  margin-right: 5px;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
  padding: 0 8px;
}

@keyframes dmAnimation {
  from {
    left: 100%;
  }

  to {
    left: -100%;
  }
}

.barrage_area {
  width: 100%;
  height: 100rem;
  position: relative;
  background: #aaa;
  overflow: hidden;
}
/* 弹幕 */
</style>
