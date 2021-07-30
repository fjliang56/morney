<template>
  <nav>
    <router-link to="/labels" class="item" active-class="selected">
      <Icon name="labels"/>标签
    </router-link>
    <router-link to="/money" class="item" active-class="selected">
      <Icon name="money"/>
      记账</router-link>
    <router-link to="/statistics" class="item" active-class="selected">
      <Icon name="statistics"/>统计
    </router-link>

  </nav>
</template>

<script lang="ts">

export default {
  name: 'Nav'
};
</script>

<style lang="scss" scoped>
nav{
  display: flex;
  flex-direction: row;
  box-shadow: 0 0 3px rgba(0,0,0,0.25);
  .item{
    padding: 8px 0 4px;
    width: 33.33333%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    font-size: 12px;
    svg {
      width: 32px; height: 32px;
    }
    p {
      margin: 4px 0;
    }
    &.selected{
      color: #42b983;
    }
  }
}

</style>