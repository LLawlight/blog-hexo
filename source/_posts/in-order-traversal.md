---
title: " 二叉树的中序遍历"
date: '2018-01-17'
categories: 
- 技术
- JavaScript
tags: 
- 技术
- JavaScript
- 算法
---

中序遍历(In-Order Traversal)，也叫做中根遍历、中序周游，指先访问左（右）子树，然后访问根，最后访问右（左）子树的遍历方式。适用于树形结构，属于深度优先遍历。

<!-- more -->

例如，有如下一种树形结构：

![](http://ww1.sinaimg.cn/large/6ad0d67fgy1fof7g9dco1j204b04ht8n.jpg)

中序遍历的序列是：a+b*c-d-e/f

---

## 实现

```js
const tree = {
  value: '-',
  left: {
    value: '+',
    left: {
      value: 'a'
    },
    right: {
      value: '*',
      left: {
        value: 'b'
      },
      right: {
        value: '-',
        left: {
          value: 'c'
        },
        right: {
          value: 'd'
        }
      }
    }
  },
  right: {
    value: '/',
    left: {
      value: 'e'
    },
    right: {
      value: 'f'
    }
  }
}
```

### JavaScript

```js
const inOrder = function (node) {
  if (node) {
    inOrder(node.left)
    console.log(node.value)
    inOrder(node.right)
  }
}

inOrder(tree)
```

得到的结果为：

![](http://ww1.sinaimg.cn/large/6ad0d67fgy1fof7gsr2waj20ll05vq2s.jpg)

符合期望！
