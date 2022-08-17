<!-- INFO 总路由，用来设置过度动画 -->
<template>
  <div id="app">
    <!-- INFO router-view 默认无法暴露内部显示的组件 -->
    <router-view class="router-view" v-slot="{ Component }">
      <transition :name="transitionName">
        <!-- NOTE 这里没有使用keep-alive 是因为对大的路由分的太细，可能有些组件不应该缓存 -->
        <component :is="Component" />
      </transition>
    </router-view>
  </div>
</template>

<script>
import { reactive, toRefs } from "vue";
import { useRouter } from "vue-router";
export default {
  setup() {
    const router = useRouter();
    const state = reactive({
      transitionName: "slide-left",
    });
    router.beforeEach((to, from) => {
      if (to.meta.index > from.meta.index) {
        state.transitionName = "slide-left"; // 向左滑动
      } else if (to.meta.index < from.meta.index) {
        // 由次级到主级
        state.transitionName = "slide-right";
      } else {
        state.transitionName = ""; // 同级无过渡效果
      }
    });

    return {
      ...toRefs(state),
    };
  },
};
</script>

<style lang="less">
html,
body {
  height: 100%;
  overflow-x: hidden;
  overflow-y: scroll;
}
#app {
  height: 100%;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  // text-align: center;
  color: #2c3e50;
}

.router-view {
  width: 100%;
  height: auto;
  position: absolute;
  top: 0;
  bottom: 0;
  margin: 0 auto;
  -webkit-overflow-scrolling: touch;
}

.slide-right-enter-active,
.slide-right-leave-active,
.slide-left-enter-active,
.slide-left-leave-active {
  height: 100%;
  will-change: transform;
  transition: all 500ms;
  position: absolute;
  backface-visibility: hidden;
}
.slide-right-enter {
  opacity: 0;
  transform: translate3d(-100%, 0, 0);
}
.slide-right-leave-active {
  opacity: 0;
  transform: translate3d(100%, 0, 0);
}
.slide-left-enter {
  opacity: 0;
  transform: translate3d(100%, 0, 0);
}
.slide-left-leave-active {
  opacity: 0;
  transform: translate3d(-100%, 0, 0);
}

.van-badge--fixed {
  z-index: 1000;
}
</style>
