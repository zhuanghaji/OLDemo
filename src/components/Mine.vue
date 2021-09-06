<template>
  <div id="m" class="shadow-box">
    <div ><input  class="iconfont user" type="text" /></div>
    <div class="iconfont psw"><input type="password" /></div>

    <button>登陆</button><button>注册</button>
  </div>
</template>

<script>
export default {
  name: "Mine",
};
</script>

<style lang='less' scoped>
#m {
  background: red;
}
.user {
    &::before {
        width: .3rem;
        content: '&#xe657;';
    }
}
</style>