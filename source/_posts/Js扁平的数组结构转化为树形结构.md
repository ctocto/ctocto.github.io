---
title: Js扁平的数组结构转化为树形结构
date: 2022-03-15 18:30:00
tags: [JavaScript,树形数据结构,数组]
categories: 饭碗(技术)
---

Js扁平的数组结构转化为树形结构，场景如：组织架构

数据源：一维数组

```js
const list = [
  { id: 2, name: '部门2', pid: 1, },
  { id: 1, name: '部门1', pid: 0, },
  { id: 3, name: '部门3', pid: 1, },
  { id: 4, name: '部门4', pid: 3, },
  { id: 5, name: '部门5', pid: 4, },
  { id: 6, name: '部门6', pid: 7, },
  { id: 7, name: '部门7', pid: 0, },
];
```
目标结果：树形结构
```json
[
  {
    "id": 1,
    "name": "部门1",
    "pid": 0,
    "children": [
      { "id": 2, "name": "部门2", "pid": 1, "children": [] },
      {
        "id": 3,
        "name": "部门3",
        "pid": 1,
        "children": [
          {
            "id": 4,
            "name": "部门4",
            "pid": 3,
            "children": [{ "id": 5, "name": "部门5", "pid": 4, "children": [] }]
          }
        ]
      }
    ]
  },
  {
    "id": 7,
    "name": "部门7",
    "pid": 0,
    "children": [{ "id": 6, "name": "部门6", "pid": 7, "children": [] }]
  }
]
```

### 方法一：
遍历
```js
function listToTree(list) {
  const result = [];
  const obj = {};
  for (const item of list) {
    const id = item.id;
    const pid = item.pid;
    if (!obj[id]) {
      obj[id] = {
        children: [],
      };
    }
    obj[id] = {
      ...item,
      children: obj[id].children,
    };

    if (pid === 0) {
      result.push(obj[id]);
    } else {
      if (!obj[pid]) {
        obj[pid] = {
          children: [],
        };
      }
      obj[pid].children.push(obj[id]);
    }
  }
  // console.log(obj)
  return result;
}

listToTree(list)
```

### 方法二
递归
```js
function listToTree(list, tree, pid) {
  list.forEach(item => {
    if (item.pid === pid) {
      const child = {
        ...item,
        children: [],
      };
      listToTree(list, child.children, item.id);

      tree.push(child);
    }
  });
}

const result = []; 
listToTree(list, result, 0);
``
