# vue-tree-shi
基于vue-tree-list更改的vue树形组件.

![vue-tree-demo.gif](https://raw.githubusercontent.com/ParadeTo/vue-tree-list/master/img/demo.gif)

[Live Demo](http://paradeto.com/vue-tree-list/)

# use
``npm install vue-tree-shi``

```javascript
<button @click="addNode">Add Node</button>
<vue-tree-list @click="onClick" :model="data" 
default-tree-node-name="new node" 
default-leaf-node-name="new leaf"
:nodeAddNewProps="nodeAddNewProps"
:nodeTextColor="nodeTextColor"
:nodeIconShow="nodeIconShow" 
@mouseOver='mouseOver1'
@extFunc="newFunc"
></vue-tree-list>
<button @click="getNewTree">Get new tree</button>
<pre>
  {{newTree}}
</pre>
...
import { VueTreeList, Tree, TreeNode } from '../src'
export default {
  components: {
    VueTreeList
  },
  data () {
    return {
      // nodeAddNewProps 新增节点/新增叶子节点，data数据新增属性，类型object
      nodeAddNewProps: {color:'',text:'12321321321312321',info:[123,444,555]},
      // nodeIconShow对象数组,对应的icon为显示或隐藏，可传入值为inline 或 none
      nodeIconShow: ['inline','inline','inline','inline','inline'],
      // nodeTextColor为传入的tree node name 文字颜色
      nodeTextColor:'#454D58',
      newTree: {},
      data: new Tree(
        [
          {
            name: '文件夹1',
            id: 1,
            pid: 0,
            dragDisabled: false,
            nodeAddNewProps:{},
            children: [
              {
                name: '文件夹11',
                id: 2,
                isLeaf: true,
                pid: 1,
                dragDisabled: false
              }
            ]
          },
          {
            name: '文件夹2',
            id: 3,
            pid: 0,
            dragDisabled: false,
            nodeAddNewProps:{
              color:'green',text:'12321321321312321',info:[123,444,555]
            },
            color:'red',
          },
          {
            name: '文件夹3',
            id: 4,
            pid: 0,
            dragDisabled: false,
            nodeAddNewProps:{},
            color:'#4242cc'
          },
          {
            name: '文件夹3',
            id: 5,
            pid: 0,
            nodeAddNewProps:{},
            dragDisabled: false
          },
          {
            name: '文件夹3',
            id: 6,
            pid: 0,
            nodeAddNewProps:{},
            dragDisabled: false
          },
          {
            name: '文件夹3',
            id: 7,
            pid: 0,
            nodeAddNewProps:{},
            dragDisabled: false
          }
        ]
      )
    }
  },
  methods: {
    addNode: function () {
      var node = new TreeNode({ name: 'new node', isLeaf: false })
      if (!this.data.children) this.data.children = []
      this.data.addChildren(node)
    },

    getNewTree: function () {
      var vm = this
      function _dfs (oldNode) {
        var newNode = {}

        for (var k in oldNode) {
          if (k !== 'children' && k !== 'parent') {
            newNode[k] = oldNode[k]
          }
        }

        if (oldNode.children && oldNode.children.length > 0) {
          newNode.children = []
          for (var i = 0, len = oldNode.children.length; i < len; i++) {
            newNode.children.push(_dfs(oldNode.children[i]))
          }
        }
        return newNode
      }

      vm.newTree = _dfs(vm.data)
    },
    // 可调用参数为当前节点Model值,节点数据
    newFunc: function (modal) {
      console.log(modal)
      alert("自定义事件1")
    },
    mouseOver1 (model) {
        // console.log(model)
    },
    onClick(model) {
      console.log(model)
    }
  }
}
```

# props
**default-tree-node-name**
 
 Default name for new treenode.

**default-leaf-node-name**

Default name for new leafnode.

# events
**click**

```javascript
<vue-tree-list @click="onClick" ...
...
onClick(model) {
  console.log(model)
}
...
```

#Forbid dragging
Use `dragDisabled` to forbid dragging:
  javascript
data: new Tree([
  {
    name: 'Node 1',
    id: 1,
    pid: 0,
    dragDisabled: true,