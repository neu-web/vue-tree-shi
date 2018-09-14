<template>
<div>
      <vue-tree-list 
          @click="onClick"
          :model="data"
          default-tree-node-name="新建节点" 
          default-leaf-node-name="新建叶子节点" 
          :nodeAddNewProps="nodeAddNewProps"
          :nodeTextColor="nodeTextColor"
          :nodeIconShow="nodeIconShow" 
          @mouseOver='mouseOver1'
          @extFunc="newFunc"
          >
      <!--
          nodeTextColor传入文字的颜色，默认为#454D58
          :nodeTextColor="nodeTextColor"
          nodeIconShow  icon是否显示，0-4对应的分别为，新建节点，新建叶子节点，编辑，删除，自定义事件
          ['inline','inline','inline','inline','inline']
          :nodeIconShow="nodeIconShow"
      -->
      </vue-tree-list>
      <button @click="addNode">test1</button>
</div>
</template>
<script>
import { VueTreeList, Tree, TreeNode } from '../dist/vue-tree-list.min'
// import { VueTreeList, Tree, TreeNode } from '../src/index'
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
    // neu shi.wy 2018-9-3  自定义按钮外部方法调用
    // 可调用参数为当前节点Model值,节点数据
    newFunc: function (modal) {
      console.log(modal)
      alert('自定义事件')
    },
    // vue-tree-list 增加节点
    addNode: function () {
      var node = new TreeNode({ name: 'new node', isLeaf: false, info:'123' })
      if (!this.data.children) this.data.children = []
      this.data.addChildren(node)
    },
    // vue-tree-list 获得更改后的data数据，all
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
    // vue-tree-list 取得当前节点的属性
    onClick (model) {
      console.log(model)
      // console.log(this.curName)
      // console.log(this.id)
      // this.curId = model.id
      // this.curName = model.name
      // this.curParent = model.pid
      // if (model.children != null) {
      //   this.curText = '有子节点'
      // } else {
      //   this.curText = '无子节点'
      // }
    },
    mouseOver1 (model) {
        // console.log(model)
      }
  }
}
</script>
<style>
 
  /* .node-content{
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
  .cm-center{
    margin-top: 40px;
    text-align: center
  }
  .sub-title{
    margin: 15px 0 20px 0;
    color:  #8898AC;
  }
  .cm-show-data{
    margin-top: 20px;
    width: 100%;
    height: 200px;
    background: rgb(228, 224, 221);
    overflow: auto;
  } */
</style>
