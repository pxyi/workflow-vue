<template>
  <div class="container">
    
    <div class="node-wrap">
      <div class="node-wrap-box">
        <div>
          <div class="title">{{ value.name }}</div>
          <div class="content">所有人 ></div>
        </div>
      </div>
      <add-node-btn :node="value" :add-node="addNode" />
    </div>

    <node-main 
      v-if="value && value.childNode" 
      :node="value.childNode" 
      :add-node="addNode"
      :delete-node="deleteNode"
    />

    <div class="end-node">
      <div class="end-node-circle"></div>
      <div class="end-node-text">流程结束</div>
    </div>

  </div>
</template>

<script>
import addNodeBtn from './add-node-btn';
import nodeMain from './node-main';
import './workflow.css';
const getRandom = () => {
  return Math.floor((Math.random() + Math.floor(Math.random() * 9 + 1)) * Math.pow(10, 9));
}
export default {
  name: 'workflow',
  components: {
    'add-node-btn': addNodeBtn,
    'node-main': nodeMain
  },
  props: {
    value: {
      type: Object,
      default: () => {}
    }
  },
  methods: {
    addNode(option) {
      this.getNode(this.value, option.nodeId, (node) => {
        /* 新增分支 */
        if (option.type === 'branch') {
          const nodeId = getRandom();
          const conditionNodes = []
          for (let i = 0; i < 2; i++) {
            conditionNodes.push({
              "name": `条件${i + 1}`,
              "type": "condition",
              "prevId": nodeId,
              "nodeId": getRandom()
            })
          }
          node.childNode = {
            type: option.type,
            prevId: node.nodeId,
            nodeId,
            conditionNodes
          }
          this.$emit('input', JSON.parse(JSON.stringify(this.value)));
        } else {
          /* 新增节点 */
          const nodeObj = {
            type: option.type,
            name: option.type === 'approver' ? '审批人' : '抄送人',
            prevId: node.nodeId,
            nodeId: getRandom()
          }
          if (node.childNode) {
            nodeObj.childNode = Object.assign({}, node.childNode);
          }
          node.childNode = nodeObj;
          this.$emit('input', JSON.parse(JSON.stringify(this.value)));
        }
      });
    },
    deleteNode(nodeId) {
      this.del(this.value, nodeId)
      this.$emit('input', JSON.parse(JSON.stringify(this.value)))      
    },
    /* 根据 nodeId 查找对应的节点并执行回调 */
    getNode(data, nodeId, call) {
      if (data.type === 'branch') {
        if (data.nodeId === nodeId) {
          call(data)
        } else {
          data.childNode && this.getNode(data.childNode, nodeId, call);
        }
        data.conditionNodes.map(d => {
          if (d.nodeId === nodeId) {
            call(d);
          } else {
            d.childNode && this.getNode(d.childNode, nodeId, call);
          }
        })
      } else {
        if (data.nodeId === nodeId) {
          call(data)
        } else {
          data.childNode && this.getNode(data.childNode, nodeId, call);
        }
      }
    },
    del(data, nodeId) {
      if (data.childNode) {
        if (data.childNode.conditionNodes) {
          for (let i in data.childNode.conditionNodes) {
            this.del(data.childNode.conditionNodes[i], nodeId);
          }
        }
        if (data.childNode.nodeId === nodeId) {
          var copy = data.childNode.childNode || null
          delete data.childNode;
          copy && (data.childNode = copy);
        } else {
          this.del(data.childNode, nodeId)
        }
      }
    },
  }
}
</script>