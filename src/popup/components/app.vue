<template>
  <div class="popup_page">
    <p class="title">
      <span class="title-text">收藏至开开阅</span>
    </p>
    <div class="setting">
      <div class="setting-item">
        <input
          type="text"
          value="160047575@qq.com"
          id="email"
          aria-label="email"
        />

        <button class="btn-diy" value="一键发送" id="send">一键发送</button>
      </div>
      <div class="setting-item">
        阅读模式:&nbsp;
        <input type="checkbox" class="switch" id="toggle" aria-label="toggle" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    function sendCR() {
      chrome.tabs.getSelected(null, function(tab) {
        myFunction(tab.url);
      });
      function myFunction(tablink) {
        const xhr = new XMLHttpRequest();
        xhr.open("post", "http://110.42.197.57:8000/collectWeb/", true);
        xhr.setRequestHeader("Content-type", "application/json");
        const obj = {
          user_email: document.getElementById("email").value,
          websit: tablink
        };
        xhr.send(JSON.stringify(obj));
      }
    }
    const sendButton = document.getElementById("send");
    sendButton.onclick = sendCR;

    function sendMessageToContentScript(message, callback) {
      chrome.tabs.query({ active: true, currentWindow: true }, function(tabs) {
        chrome.tabs.sendMessage(tabs[0].id, message, response => {
          if (callback) callback(response);
        });
      });
    }
    function toggleCR() {
      sendMessageToContentScript({
        value: toggleButton.checked,
        method: "readMode"
      });
    }
    const toggleButton = document.getElementById("toggle");
    sendMessageToContentScript(
      { value: "get_status", method: "readModeOn" },
      response => {
        toggleButton.checked = ["true", true].includes(response)
          ? "checked"
          : null;
      }
    );
    toggleButton.onchange = toggleCR;
  }
};
</script>

<style lang="less">
.popup_page {
  color: red;
  .popup_page_main {
    color: green;
  }
}
.popup_page {
  font-family: -apple-system, BlinkMacSystemFont, Helvetica Neue, PingFang SC,
    Microsoft YaHei, Source Han Sans SC, Noto Sans CJK SC, WenQuanYi Micro Hei,
    sans-serif;
  font-size: 12px;
  width: 300px;
  /* height: 200px; */
  padding: 10px;
  overflow: hidden;
  color: #222;
  background-color: #f4f4f4;
  box-sizing: border-box;
}

body * {
  box-sizing: inherit;
  margin: 0;
  padding: 0;
}

.title {
  font-size: 18px;
  font-weight: bolder;
  padding-bottom: 10px;
  display: flex;
  justify-content: space-between;
}

.logo {
  width: 24px;
  margin-right: 6px;
  vertical-align: text-top;
}

.title-text {
  font-size: 16px;
  margin-right: auto;
}

.setting {
  width: 100%;
  padding: 10px;
  background-color: #fff;
  border-radius: 6px;
  font-size: 14px;
}

.setting-item {
  padding: 8px 0;
  justify-content: center;
  align-items: center;
}

input {
  vertical-align: bottom;
  height: 30px;
}

.btn-diy {
  height: 30px;
  display: inline-block;
  font-weight: 400;
  line-height: 1.5;
  color: #212529;
  text-align: center;
  text-decoration: none;
  vertical-align: middle;
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  user-select: none;
  background-color: transparent;
  border: 1px solid transparent;
  padding: 0.2rem 0.5rem;
  border-radius: 0.25rem;
  transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out,
    border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
  color: #0d6efd;
  border-color: #0d6efd;
  margin-left: 5px;
}

.btn-diy:hover {
  color: #fff;
  background-color: #0d6efd;
  border-color: #0d6efd;
}

/* Switch开关样式 */
input[type="checkbox"].switch {
  outline: none;
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  position: relative;
  width: 40px;
  height: 20px;
  background: #ccc;
  border-radius: 10px;
  transition: border-color 0.3s, background-color 0.3s;
}

input[type="checkbox"].switch::after {
  content: "";
  display: inline-block;
  width: 1rem;
  height: 1rem;
  border-radius: 50%;
  background: #fff;
  box-shadow: 0, 0, 2px, #999;
  transition: 0.4s;
  top: 2px;
  position: absolute;
  left: 2px;
}

input[type="checkbox"].switch:checked {
  background: #0d6efd;
}

/* 当input[type=checkbox]被选中时：伪元素显示下面样式 位置发生变化 */
input[type="checkbox"].switch:checked::after {
  content: "";
  position: absolute;
  left: 55%;
  top: 2px;
}
</style>
