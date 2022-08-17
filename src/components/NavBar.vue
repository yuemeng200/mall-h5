<template>
  <div class="nav-bar van-hairline--top">
    <ul class="nav-list">
      <router-link class="nav-list-item active" to="home">
        <i class="nbicon nblvsefenkaicankaoxianban-1"></i>
        <span>首页</span>
      </router-link>
      <router-link class="nav-list-item" to="category">
        <i class="nbicon nbfenlei"></i>
        <span>分类</span>
      </router-link>
      <router-link class="nav-list-item" to="cart">
        <van-icon name="shopping-cart-o" :badge="!count ? '' : count" />
        <span>购物车</span>
      </router-link>
      <router-link class="nav-list-item" to="user">
        <i class="nbicon nblvsefenkaicankaoxianban-"></i>
        <span>我的</span>
      </router-link>
    </ul>
  </div>
</template>

<script>
import { onMounted, computed } from "vue";
import { useRoute } from "vue-router";
import { useStore } from "vuex";
import { getLocal } from "@/common/js/utils";
export default {
  setup() {
    const route = useRoute();
    const store = useStore();
    onMounted(() => {
      const token = getLocal("token");
      const path = route.path;
      // INFO 登录且有必要去获取下购物车数量
      if (token && !["/home", "/category"].includes(path)) {
        store.dispatch("updateCart");
      }
    });
    // NOTE 这里我感觉没必要，因为这个navbar组件太容易切换
    const count = computed(() => {
      return store.state.cartCount;
    });

    return {
      count,
    };
  },
};
</script>

<style lang="less" scoped>
@import "../common/style/mixin";
.nav-bar {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;  // INFO 定位元素必须设置width
  padding: 5px 0;
  z-index: 1000;
  background: #fff;
  .nav-list {
    .fj();
    .nav-list-item {
      display: flex;
      flex: 1;
      flex-direction: column;
      text-align: center;
      color: #666;
      &.router-link-active {
        color: @primary;
      }
      i {
        text-align: center;
        font-size: 22px;
      }
      span {
        font-size: 12px;
      }
      .van-icon-shopping-cart-o {
        margin: 0 auto;
        margin-bottom: 2px;
      }
    }
  }
}
</style>
