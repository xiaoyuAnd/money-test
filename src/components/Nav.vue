<template>
  <nav>
    <router-link to="/money" class="item" active-class="selected">
      <Icon name="money" />
      记账
    </router-link>
    <router-link to="/labels" class="item" active-class="selected">
      <Icon name="label" />
      标签
    </router-link>
    <router-link to="/statistics" class="item" active-class="selected">
      <Icon name="statistics" />
      明细
    </router-link>
  </nav>
</template>

<script>
export default {};
</script>

<style scoped lang="scss">
nav {
  display: flex;
  flex-direction: row;
  box-shadow: 0 0 3px rgba(0, 0, 0, 0.5);
  font-size: 12px;
  padding: 4px 0;
  > .item {
    padding: 2px 0;
    width: 33.333%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .icon {
    width: 34px;
    height: 34px;
  }

  .item.selected {
    color: orange;
  }
}
</style>
