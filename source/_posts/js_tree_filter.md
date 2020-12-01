---
title: js 递归过滤树形（tree）结构
date: 2020-12-01 15:23:09
tags: [JavaScript,js,递归,过滤,filter,tree]
categories: 饭碗(技术)
---

# 问题
一个树型结构的数据，过滤掉不符合条件的节点，及其父节

# 示例
过滤掉 type: 'page' 没有page 信息的 节点, 如果树干节点下没有一个符合条件的叶子节点那么就一同过滤掉

```js
var arr = [
  {
    id: 1,
    type: 'group',
    children: [
      {
        id: 11,
        type: 'group',
        children: [
          {
            id: 111,
            type: 'page',
            page: {
              name: 'page-111',
            }
          }
        ]
      },
      {
        id: 12,
        type: 'group',
        children: [
          {
            id: 121,
            type: 'page',
            page: {
              name: 'page-121',
            }
          },
          {
            id: 122,
            type: 'page',
          }
        ]
      }
    ]
  },
  {
    id: 2,
    type: 'group',
    children: [
      {
        id: 21,
        type: 'group',
        children: [
          {
            id: 211,
            type: 'page',
          }
        ]
      },
      {
        id: 22,
        type: 'group',
        children: [
          {
            id: 221,
            type: 'group',
            children: [
              {
                id: 2221,
                type: 'page',
              }
            ]
          }
        ]
      },
      {
        id: 23,
        type: 'group',
        children: [
          {
            id: 231,
            type: 'page'
          }
        ]
      }
    ]
  },
  {
    id: 3,
    type: 'group',
  },
  {
    id: 4,
    type: 'group',
    children: [
      {
        id: 41,
        type: 'page',
        page: {
          name: 'page-41',
        }
      }
    ]
  }
]


function filterNoPageMenu(nav) {
  return nav.filter(item => {
    if (item.type === 'page' && item.page) {
      return true
    } else if (item.type === 'group' && item.children && item.children.length) {
      const res = filterNoPageMenu(item.children)
      if (res.length) {
        item.children = res
        return true
      }
    }
    return false
  })
}
console.log(JSON.parse(JSON.stringify(arr)))
console.log(filterNoPageMenu(arr))
```

