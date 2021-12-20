---
title: 简化版类jQuery的js事件委托
date: 2021-12-20 19:40:00
tags: [JavaScript,js,event,delegate,on,off,事件委托]
categories: 饭碗(技术)
---


```js
// event.js
const matches = (element, query) => {
  const matched = (element.document || element.ownerDocument).querySelectorAll(
    query,
  )
  let i = matched.length - 1
  while (i >= 0 && matched.item(i) !== element) {
    i -= 1
  }
  return i > -1
}

const findAncestor = (element, selector) => {
  if (typeof element.closest === 'function') {
    return element.closest(selector) || null
  }
  while (element) {
    if (matches(element, selector)) {
      return element
    }
    // eslint-disable-next-line no-param-reassign
    element = element.parentElement
  }
  return null
}

const listenerList = []

/**
 * On
 * @param {EventTarget} element Element，document, window...
 * @param {String} query Descendant element classname or id
 * @param {String} eventNames Event type eg: click
 * @param {Function} fn callback
 * @param {Boolean} capture 
 */
export const on = (element, query, eventNames, fn, capture = false) => {
  const events = eventNames.split(' ')
  events.forEach((event) => {
    const listener = (e) => {
      const delegateTarget = findAncestor(e.target, query)
      if (delegateTarget) {
        e.delegateTarget = delegateTarget
        fn(e)
      }
    }
    listenerList.push({ listener, element, query, event, capture })
    element.addEventListener(event, listener, capture)
  })
}

/**
 * Off
 * @param {EventTarget} element Element，document, window...
 * @param {String} query Descendant element classname or id
 * @param {String} eventNames Event type A boolean value indicating that events of this type will be dispatched to the registered listener before being dispatched to any EventTarget beneath it in the DOM tree.
 */
export const off = (element, query, eventNames) => {
  const events = eventNames.split(' ')
  events.forEach((event) => {
    listenerList.forEach((item, index) => {
      if (
        item.element === element &&
        item.query === query &&
        item.event === event
      ) {
        element.removeEventListener(event, item.listener, item.capture)
        listenerList.splice(index, 1)
      }
    })
  })
}

/**
 * Trigger
 * @param {EventTarget} element Element，document, window...
 * @param {*} event A string representing the name of the event.
 * @param {*} args An object, with the following properties: detail
 */
export const trigger = (element, event, args) => {
  const ev = new CustomEvent(event, args)
  element.dispatchEvent(ev)
}

```

使用

```js
import * as Event from './event.js';

const container = document.querySelector('.list')
Event.on(container, '.item', 'click', () => {
  console.log('Click');
})
```

