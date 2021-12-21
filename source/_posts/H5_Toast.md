---
title: 移动H5 Toast 提示组建
date: 2021-12-21 19:30:00
tags: [H5,JavaScript,CSS,Toast,提示框,组建]
categories: 饭碗(技术)
---
es6 + less 实现的移动web端信息提示组建

<img src="https://raw.githubusercontent.com/ctocto/ctocto.github.io/develop/images/72643c84-acd9-4204-8ba3-d892abebef24.png" alt="Toast loading" width="280" /><img src="https://raw.githubusercontent.com/ctocto/ctocto.github.io/develop/images/ba65a6ad-4b59-40ab-9dec-402364ba1d43.png" alt="Toast Info" width="280" />

```js
// Toast.js
import './toast.less';

class Toast {
  body = document.body;

  constructor(content, type, duration = 2, onClose = () => {}) {
    this.content = content;
    this.type = type;
    this.duration = duration;
    this.onClose = onClose;

    this.start();
  }

  start() {
    const { type } = this;
    switch (type) {
      case 'info':
        this.renderInfo();
        break;

      case 'loading':
        this.renderLoading();
        break;
      default:
        this.renderInfo();
        break;
    }
  }

  static info(content, duration, onClose) {
    const text = document.createTextNode(content);
    return new Toast(text, 'info', duration, onClose);
  }

  static loading(content, duration = 0, onClose) {
    const dom = document.createElement('div');
    dom.setAttribute('class', 'lib-toast-loading');
    let appendContent = dom;
    if (content) {
      const text = document.createElement('div');
      text.setAttribute('class', 'lib-toast-loading-text');
      text.textContent = content;

      appendContent = [dom, text];
    }

    return new Toast(appendContent, 'loading', duration, onClose);
  }

  hide() {
    this.toast.removeEventListener('touchmove', this.refuseTouch, false);
    this.toast.remove();
    this.toast = null;
  }

  renderInfo() {
    const { content } = this;
    this.render(content);
  }

  renderLoading() {
    const { content } = this;
    this.render(content);
  }

  refuseTouch(e) {
    e.stopPropagation();
    e.preventDefault();
  }

  render(children) {
    const baseCSS =
      'transition: transform 200ms, opacity 200ms;transition-timing-function: cubic-bezier(0.250, 0.460, 0.450, 0.940);transform: scale(0);opacity: 0;';
    this.toast = document.createElement('div');
    this.toast.setAttribute('class', 'lib-toast-wrapper');
    const padding = document.createElement('div');
    padding.setAttribute('class', 'li-toast-padding');
    const block = document.createElement('div');
    padding.appendChild(block);
    block.setAttribute('class', 'lib-toast-block');
    block.style.cssText = baseCSS;
    if (Array.isArray(children)) {
      block.append(...children);
    } else {
      block.appendChild(children);
    }
    this.toast.appendChild(padding);
    this.body.appendChild(this.toast);
    this.toast.addEventListener('touchmove', this.refuseTouch, false);
    // start animation
    // requestAnimationFrame(() => {
    //   // block.style.cssText = `${baseCSS} transform: scale(1); opacity: 1;`
    // })
    setTimeout(() => {
      block.style.cssText = `${baseCSS} transform: scale(1); opacity: 1;`;
    }, 0);

    if (this.duration) {
      setTimeout(() => {
        this.hide();
      }, this.duration * 1000);
    }

    return this.toast;
  }
}

export default Toast;
```
```less
// toast.less
.lib-toast-wrapper {
  position: fixed;
  z-index: 9999;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  user-select: none;
  display: table;
  text-align: center;
  vertical-align: middle;

  .li-toast-padding {
    display: table-cell;
    height: 100%;
    width: 100%;
    text-align: center;
    vertical-align: middle;
  }

  .lib-toast-block {
    display: inline-block;
    padding: 0.5em 0.9em;
    border-radius: 4px;
    background: rgba(0, 0, 0, 0.75);
    color: #ffffff;
    vertical-align: middle;
  }
  .lib-toast-loading {
    margin: 0.4em auto;
    width: 2em;
    height: 2em;
    border-radius: 2em;
    border: 0.4em solid rgba(255, 255, 255, 0.4);
    border-top-color: #ffffff;
    animation: loading 1.2s ease 0s infinite reverse backwards;
  }
  .lib-toast-loading-text {
    margin-top: 0.3em;
   }
}
@keyframes loading {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(-360deg); }
}
```
### 使用
1、信息提示
```js
import Toast from './Toas.js';

Toast.info('提交成功');
```

2、加载提示
```js
const msg = message.loading('加载中...'); // message.loading();
setTimeout(() => {
  msg.hide();
}, 1000);
```
    
    
    
    
    
    
    
