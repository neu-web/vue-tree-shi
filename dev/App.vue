<template>
<div>

      <vue-tree-list 
          @click="onClick"
          :model="data"
          default-tree-node-name="新建节点" 
          default-leaf-node-name="新建叶子节点" 
          :shiTextColor="shiTextColor"
          :shiIconColor="shiIconColor" 
          @mouseOver='mouseOver1'
          @shiFuc="newFunc"
          >
      <!--
          shiTextColor传入文字的颜色，默认为#454D58
          :shiTextColor="shiTextColor"
          shiIconColor  icon是否显示，0-4对应的分别为，新建节点，新建叶子节点，编辑，删除，自定义事件
          ['inline','inline','inline','inline','inline']
          :shiIconColor="shiIconColor"
      -->
      </vue-tree-list>

      <!-- <div class="cm-center">
          <el-button type="primary" class=“buttonOne” @click="addNode">
            新增同级
          </el-button>
          <el-button type="primary" name="button" @click="getNewTree">
            当前data数据
          </el-button>
          <el-button type="primary" name="button" >
            保存
          </el-button>
      </div> -->
      <div>
      <pre class="cm-show-data">
        {{newTree}}
      </pre>
      </div>
</div>
</template>

<script>
// import { VueTreeList, Tree, TreeNode } from '../dist/vue-tree-list.min'
import { VueTreeList, Tree, TreeNode } from '../src/index'

export default {
  components: {
    VueTreeList
  },
  data () {
    return {
      // shiIconColor对象数组,对应的icon为显示或隐藏，可传入值为inline 或 none
      shiIconColor: ['inline','inline','inline','inline','inline'],
      // shiTextColor为传入的tree node name 文字颜色
      shiTextColor:'#454D58',
      newTree: {},
      data: new Tree([
        {
          name: '文件夹1',
          id: 1,
          pid: 0,
          info: {
            info1: 'text1',
            info2: 'text2'},
          dragDisabled: true,
          children: [
            {
              name: '文件夹11',
              id: 2,
              isLeaf: true,
              pid: 1,
              info: {
                info1: 'text1',
                info2: 'text2'}
            }
          ]
        },
        {
          name: '文件夹2',
          id: 3,
          pid: 0,
          dragDisabled: true
        },
        {
          name: '文件夹3',
          id: 4,
          pid: 0
        },
        {
          name: '5555',
          id: 5,
          pid: 0
        },
        {
          name: '6666',
          id: 6,
          pid: 0
        },
        {
          name: '777',
          id: 7,
          pid: 0
        }
      ])
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
      var node = new TreeNode({ name: 'new node', isLeaf: false })
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
      // console.log(model)
      // console.log(this.curName)
      // console.log(this.id)
      this.curId = model.id
      this.curName = model.name
      this.curParent = model.pid
      if (model.children != null) {
        this.curText = '有子节点'
      } else {
        this.curText = '无子节点'
      }
    },
    mouseOver1 (model) {
        // console.log(model)
      }
  }
}
</script>
<style scoped>
  .node-content{
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
  }
</style>
