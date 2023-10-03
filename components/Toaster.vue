<template>
  <ul class="notifications">
    <li v-for="(toast, index) in toasts" :key="index" :class="toast.type">
      <div class="column">
        <i class="fa-solid" :class="toast.icon"></i>
        <span>{{ toast.text }}</span>
      </div>
      <i class="fa-solid fa-xmark" @click="removeToast(index)"></i>
    </li>
  </ul>
  <div class="buttons">
    <button class="btn" @click="showToast('success')">Success</button>
    <button class="btn" @click="showToast('error')">Error</button>
    <button class="btn" @click="showToast('warning')">Warning</button>
    <button class="btn" @click="showToast('info')">Info</button>
    <button class="btn" @click="showToast('upload')">Upload</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const toasts = ref([]);

const toastDetails = {
  timer: 10000,
  success: {
    icon: 'arrive-dynamically-success',
    text: 'Success: This is a success toast.',
  },
  error: {
    icon: 'arrive-dynamically-error',
    text: 'Error: This is an error toast.',
  },
  warning: {
    icon: 'arrive-dynamically-warning',
    text: 'Warning: This is a warning toast.',
  },
  info: {
    icon: 'arrive-dynamically-info',
    text: 'Info: This is an information toast.',
  },
  upload: {
    icon: 'arrive-dynamically-upload',
    text: 'Upload: File uploading',
  },
};

const showToast = (id) => {
  const { icon, text } = toastDetails[id];
  toasts.value.push({ type: id, icon, text });
  const toast = toasts.value[toasts.value.length - 1];
  setTimeout(() => removeToast(toast), toastDetails.timer);
};

const removeToast = (toast) => {
  toasts.value = toasts.value.filter((t) => t !== toast);
};
</script>

<style lang="scss" scoped>
$dark: #34495E;
$light: #ffffff;
$success: #0ABF30;
$error: #E24D4C;
$warning: #E9BD0C;
$info: #3498DB;
$upload: #17e39b;
$bg: rgb(229, 226, 226);

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
//:root {
//  --dark: #34495E;
//  --light: #ffffff;
//  --success: #0ABF30;
//  --error: #E24D4C;
//  --warning: #E9BD0C;
//  --info: #3498DB;
//  --upload: #17e39b;
//  --bg: rgb(229, 226, 226);
//}
body {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: $light;
}
li {
  list-style-type: none;
  height: 50px;
  width: 300px;
  margin-top: 15px;
  border-radius: 6px;
  padding: 14px 0 0 5px;
  background: lightgrey;
}
.notifications {
  position: fixed;
  top: 30px;
  right: 20px;
}
.notifications :where(.toast, .column) {
  display: flex;
  align-items: center;
}
.notifications .toast {
  width: 400px;
  position: relative;
  overflow: hidden;
  list-style: none;
  border-radius: 4px;
  padding: 16px 17px;
  margin-bottom: 10px;
  background: $bg;
  color: $dark;
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
  animation: progress 5s linear forwards;
}
@keyframes progress {
  100% {
    width: 0%;
  }
}
.toast.success::before, .btn#success {
  background: $success;
}
.toast.error::before, .btn#error {
  background: $error;
}
.toast.warning::before, .btn#warning {
  background: $warning;
}
.toast.info::before, .btn#info {
  background: $info;
}
.toast.upload::before, .btn#upload {
  background: $upload;
}
.toast .column i {
  font-size: 1.75rem;
}
.toast.success .column i {
  color: $success;
}
.toast.error .column i {
  color: $error;
}
.toast.warning .column i {
  color: $warning;
}
.toast.info .column i {
  color: $info;
}
.toast.upload .column i {
  color: $upload;
}
.toast .column span {
  font-size: 1.07rem;
  margin-left: 12px;
}
.toast i:last-child {
  color: #aeb0d7;
  cursor: pointer;
}
.toast i:last-child:hover {
  color: $bg;
}
.buttons .btn {
  border: none;
  outline: none;
  cursor: pointer;
  margin: 0 5px;
  color: $dark;
  font-size: 1.2rem;
  padding: 10px 20px;
  border-radius: 4px;

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
