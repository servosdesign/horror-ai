<template>
  <div class="example">
    <span class="smooth spinner" />
  </div>
</template>

<script>

</script>

<style scoped>
.example {
  position: relative;
  background-color: transparent;
}

.spinner::after {
  content: '';
  box-sizing: border-box;
  width: 15px;
  height: 15px;
  position: absolute;
  top: calc(50% - 20px);
  left: calc(50% - 20px);
  border-radius: 50%;
}

.spinner.smooth::after {
  border-top: 2px solid rgb(0, 0, 0);
  border-left: 2px solid rgb(0, 0, 0);
  border-right: 2px solid rgba(0, 0, 0, 0);
  animation: spinner .8s linear infinite;
}

@keyframes spinner {
  to {transform: rotate(360deg);}
}
</style>
