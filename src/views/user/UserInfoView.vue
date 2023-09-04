<template>
  <div id="about">
    <a-form :style="{ width: '400px' }" class="ant-form">
      <span style="font-size: 20px">待开发</span>
    </a-form>
  </div>
</template>
<style>
#about {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

#about .ant-form {
  background: rgba(255, 255, 255, 0.5);
  border-radius: 15px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  background-clip: padding-box;
  padding: 100px;
  margin-top: 100px;
}

#about .ant-form-item {
  margin-top: 30px;
  margin-right: 30px;
}

#about .ant-input {
  border-radius: 15px;
  background: rgba(255, 255, 255, 0.5);
  box-shadow: 0 0 1px rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(10px);
  background-clip: padding-box;
}
</style>
