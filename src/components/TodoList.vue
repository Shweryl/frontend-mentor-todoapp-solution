<template>
  <div class="container-box">
    <div class="background-wrap"></div>
    <div class="todo-wrap">
      <div class="todo-inner">
        <div class="header-theme">
        <h1 class="header">TODO</h1>
        <img src="" alt="" class="theme-toggle">
      </div>
      <div class="input-box"></div>
      <div class="list-box"></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
    

}
</script>

<style lang="scss">
.container-box{
  width : 100%;
  height : 100vh;

  .background-wrap{
    height : 40%;
    background-image: url("@/assets/images/bg-desktop-light.jpg");
    background-color: hsl(0, 0%, 98%);
  }

  .todo-wrap{
    display: flex;
    justify-content: center;
    align-items: center;
    
    .todo-inner{
      width : 100%;
      max-width : 520px;
      background-color: red;
      padding : 10px;
    }
  }
}
</style>