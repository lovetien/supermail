<template>
    <div id="tab-bar">
     <slot></slot>
    </div>
</template>

<script>
    export default {
        name:"TabBar"
    }
</script>

<style scoped>
   #tab-bar{
    display: flex;
    background-color: #f3f3f3;
    position: fixed;
    right:0;
    left: 0;
    bottom: 0;
    box-shadow: 0 -3px 1px rbga(100,100,100,.3);

  }
  
</style>