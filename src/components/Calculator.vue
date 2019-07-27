<template>
  <div class="wrapper">
    <div class="cal-box">
      <div class="cal-header">

      </div>
      <div class="cal-body"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator'
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import '../scss/_mixin.scss';
.wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
}
.cal-box {
  width: 350px;
  height: 525px;
}
.cal-header {
  height: 110px;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  background-color: $darkBlue;
}
.cal-body {
  height: 415px;
  background-color: $primaryBlue;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
}
</style>
