<template>
  <div class="container">
    <ul class="notifications">
      <li v-for="(toast, index) in toasts" :key="index" :class="['toast', { 'hide': toast.hide }, toast.type.class]" >
        <div class="column">
          <div class="icon">
            <i class="fa-solid" :class="toast.icon">i</i>
          </div>
          <div class="list-text">
            <div><b>{{ toast.type.class }}</b></div>
            <p>{{ toast.text }}</p>
          </div>
        </div>
        <span @click="removeToast(toast)">x</span>
      </li>
    </ul>
    <div class="buttons">
      <button class="btn" id="success" @click="showToast('Success')">Success</button>
      <button class="btn" id="error" @click="showToast('Error')">Error</button>
      <button class="btn" id="warning" @click="showToast('Warning')">Warning</button>
      <button class="btn" id="info" @click="showToast('Info')">Info</button>
      <button class="btn" id="upload" @click="showToast('Upload')">Upload</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const toasts = ref([]);

const toastDetails = {
  // timer: 10000,
  Success: {
    icon: 'arrive-dynamically-success',
    text: 'This is a success toast.',
    timer: 2000,
  },
  Error: {
    icon: 'arrive-dynamically-error',
    text: 'This is an error toast.',
    timer: 5000,
  },
  Warning: {
    icon: 'arrive-dynamically-warning',
    text: 'This is a warning toast.',
    timer: 7000,
  },
  Info: {
    icon: 'arrive-dynamically-info',
    text: 'This is an information toast.',
    timer: 5000,
  },
  Upload: {
    icon: 'arrive-dynamically-upload',
    text: 'File uploading...',
    timer: 10000,
  },
};
const timerAnimation = ref();
// const temTimer = ref('');
const showToast = (id) => {
  const { icon, text } = toastDetails[id];
  toasts.value.push({ type: { class: id }, icon, text });
  const toast = toasts.value[toasts.value.length - 1];
  timerAnimation.value = `${toastDetails[id].timer/1000}s`
  console.log(timerAnimation.value);
  setTimeout(() => {
    // timerAnimation.value='';
    removeToast(toast);
  }, toastDetails[id].timer);
};
const removeToast = (toast) => {
  toast.hide = true;
  setTimeout(() => {
    toasts.value = toasts.value.filter((t) => t !== toast);
  }, 300);
};
const getAnimationDuration = (id) => {
  return `${toastDetails[id].timer / 1000}s`;
};
</script>

<style scoped>

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
:root {
  --dark: #34495E;
  --light: #ffffff;
  --success: #0ABF30;
  --error: #E24D4C;
  --warning: #E9BD0C;
  --info: #3498DB;
  --upload: #17e39b;
  --bg: lightgray;
}
body {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  //background: var(--light);
}
.notifications {
  position: fixed;
  top: 30px;
  right: 20px;
}
.column{
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.icon{
  margin-right: 1.5rem;
  border-radius: 10px;
  border: 1px solid black;
}

.list-text{
  display: flex;
  flex-direction: column;
}
.list-text div{
  font-size:17px;
}
.notifications :where(.toast, .column) {
  display: flex;
  align-items: center;
}
.notifications .toast {
  width: 300px;
  //height: 80px;
  position: relative;
  overflow: hidden;
  list-style: none;
  border-radius: 4px;
  padding: 16px 17px;
  margin-bottom: 10px;
  background: #ffffff;
  box-shadow: 0 6px 20px -5px rgba(0, 0, 0, 0.1);
  //color: var(--dark);
  justify-content: space-between;
  animation: show_toast 0.3s ease forwards;
}
@keyframes show_toast {
  0% {
    transform: translateX(100%);
  }
  40% {
    transform: translateX(-5%);
  }
  80% {
    transform: translateX(0%);
  }
  100% {
    transform: translateX(-10px);
  }
}
.notifications .toast.hide {
  animation: hide_toast 0.3s ease forwards;
}
@keyframes hide_toast {
  0% {
    transform: translateX(-10px);
  }
  40% {
    transform: translateX(0%);
  }
  80% {
    transform: translateX(-5%);
  }
  100% {
    transform: translateX(calc(100% + 20px));
  }
}
.toast::before {
  position: absolute;
  content: "";
  height: 3px;
  width: 100%;
  bottom: 0px;
  left: 0px;
  animation: progress linear forwards;
  animation-duration: v-bind(timerAnimation);
}
@keyframes progress {
  100% {
    width: 0%;
  }
}
.toast.Success::before, .btn#success {
  background: #0ABF30;
}
.toast.Error::before, .btn#error {
  background: #E24D4C;
}
.toast.Warning::before, .btn#warning {
  background: #E9BD0C;
}
.toast.Info::before, .btn#info {
  background: #3498DB;
}
.toast.Upload::before, .btn#upload {
  background: #17e39b;
}
.toast .columns i {
  font-size: 1.75rem;
}
.toast.Success .column i {
  color: #0ABF30;
}
.toast.Error .column i {
  color: #E24D4C;
}
.toast.Warning .column i {
  color: #E9BD0C;
}
.toast.Info .column i {
  color: #3498DB;
}
.toast.Upload .column i {
  color: #17e39b;
}
.toast .column span {
  font-size: 1.07rem;
  margin-left: 12px;
}
.toast span {
  cursor: pointer;
  background: lightgrey;
  border-radius: 5px;
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.toast span:hover {
  color: red;
}
.buttons .btn {
  border: none;
  outline: none;
  cursor: pointer;
  margin: 0 5px;
  color: #34495E;
  font-size: 1.2rem;
  padding: 10px 20px;
  border-radius: 4px;
  background: #E9BD0C;
}
@media screen and (max-width: 530px) {
  .notifications {
    width: 95%;
  }
  .notifications .toast {
    width: 100%;
    font-size: 1rem;
    margin-left: 20px;
  }
  .buttons .btn {
    margin: 0 1px;
    font-size: 1.1rem;
    padding: 8px 15px;
  }
}
</style>
