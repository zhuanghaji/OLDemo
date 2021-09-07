<template>
  <div class="shadow-box mine-layout">
    <div class="input-layout user">
      <span class="iconfont">&#xe657;</span><input type="text" />
    </div>
    <div class="input-layout psw">
      <span class="iconfont">&#xe71c;</span><input type="password" />
    </div>

    <button>登陆</button><button>注册</button>
  </div>
</template>

<script>
export default {
  name: "Mine",
};
</script>

<style lang='less' scoped>
.input-layout {
  display: flex;
  flex-direction: row;
  border: rgb(212, 212, 212) solid 1px;
  border-radius: 0.3rem;
  align-items: center;
  height: 2rem;
  padding: 0 5px;
  margin-bottom: 0.6rem;

  input {
    outline: none;
    border: none;
    margin-left: 0.5rem;
  }
}
.mine-layout {
  position: absolute;
  right: 3.8rem;
  background: white;
  top: 9rem;
  opacity: 0;
  box-sizing: border-box;
  padding: 0.3rem;
  transition: 0.5s;
}
</style>