<template>
  <div id="nav">
    <ul class="nav-ul">
      <li
        @click="itemClick('home')"
        class="nav-item shadow-box"
        :class="{ 'nav-selected': selectedType == 'home' }"
      >
        <span class="iconfont">&#xe6cb;</span><span class="nav-text">首页</span>
      </li>
      <li
        @click="itemClick('journey')"
        class="nav-item shadow-box"
        :class="{ 'nav-selected': selectedType == 'journey' }"
      >
        <span class="iconfont">&#xe6ec;</span><span class="nav-text">行程</span>
      </li>
      <li
        @click="itemClick('mine')"
        class="nav-item shadow-box"
        :class="{ 'nav-selected': selectedType == 'mine' }"
      >
        <span class="iconfont">&#xe657;</span><span class="nav-text">我</span>
      </li>
    </ul>
    <Mine id="mine" :style="{ opacity: selectedType == 'mine' ? 1 : 0 }" />
  </div>
</template>


<script>
import Mine from "./Mine.vue";
export default {
  name: "Nav",
  components: { Mine },
  data() {
    return {
      selectedType: "mine",
    };
  },
  methods: {
    itemClick(type) {
      this.selectedType = type;
    },
  },
};
</script>

<style lang='less' scoped>
.iconfont {
  font-size: 2rem !important;
}
#mine {
  position: absolute;
  right: 3.8rem;
  background: white;
  top: 9rem;
  opacity: 0;
  box-sizing: border-box;
  padding: .3rem;
  transition: .5s;
}
.nav-item {
  margin-bottom: 1rem;
  height: 3.5rem;
  width: 3.5rem;
  display: flex;
  background: white;
  flex-direction: column;
  align-items: center;
}
.nav-selected {
  background: rgb(123, 123, 238);
  color: white;
  .nav-text {
    color: white;
  }
}

.nav-text {
  font-size: 0.8rem;
  color: gray;
}
</style>