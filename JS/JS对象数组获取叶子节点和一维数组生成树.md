# JS对象数组获取叶子节点和一维数组生成树

## 对象数组扁平化

给定一个对象数组和要属性，获取所有叶子节点。

## 例子

```javascript
// 根据属性获取叶子节点
function getListLeafNode (list, prop, tmpList = []) {
    for (let i = 0; i < list.length; i++) {
        if (list[i][prop] && list[i][prop].length) {
            this.getListLeafNode(list[i][prop], prop, tmpList)
        } else {
            tmpList.push(list[i])
        }
    }
    return tmpList
}

let arr = [{
    id: '1',
    children: [{
        id: 11
    }, {
        id: 12,
        children: [{
            id: 123
        }]
    }]
}, {
    id: '2'
}]

getListLeafNode(arr, 'children')
```

## 一维数组生成树

根据属性将一维数组连结成树结构。

## 例子

```javascript
function createTree (items, id = null, link = 'pid') {
    return items
    .filter(item => item[link] === id) // 获取父层层节点
    .map(item => ({ ...item, children: createTree(items, item.id) }))
}

let arr = [{
    id: '1',
    pid: null
}, {
    id: '2',
    pid: null
}, {
    id: '11',
    pid: '1'
}, {
    id: '111',
    pid: '11'
}]

createTree(arr)
```

