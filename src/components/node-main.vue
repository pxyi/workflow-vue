<template>
  <div v-if="node && node.nodeId && node.childNode && node.conditionNodes" class="branch-and-node-wrap"> 
    <div class="branch-wrap">
      <div class="branch-wrap-box">
        <div class="branch-box">
          <button class="add-branch" @click="addBranch(node)">添加条件</button>
          <col-box 
            v-for="(branch, idx) in node.conditionNodes" 
            :key="branch.nodeId" 
            :index="idx" 
            :node="branch"
            :add-node="addNode" :delete-node="deleteNode"
            :parentLength="node.conditionNodes.length" 
            @delbranch="delBranch(node, $event)"
          />
        </div>
        <add-node-btn :node="node" :add-node="addNode" :delete-node="deleteNode" />
      </div>
    </div>
    <div v-if="node.childNode.type === 'branch'">
      <node-main :node="node.childNode"   :add-node="addNode" :delete-node="deleteNode" />
    </div>
    <div v-else class="node-wrap">
      <node-wrap-box :node="node.childNode" :delete-node="deleteNode" />
      <add-node-btn :node="node.childNode" :add-node="addNode" :delete-node="deleteNode" />
      <node-main 
        v-if="node.childNode.childNode" 
        :node="node.childNode.childNode"
        :add-node="addNode" 
        :delete-node="deleteNode" 
      />
    </div>
  </div>
  <div v-else-if="node && node.nodeId" :class="[node.type === 'branch' ? 'branch-wrap' : 'node-wrap']">
    <div v-if="node.type === 'branch'" class="branch-wrap-box">
      <div class="branch-wrap-box">
        <div class="branch-box">
          <button class="add-branch" @click="addBranch(node)">添加条件</button>
          <col-box 
            v-for="(branch, idx) in node.conditionNodes" 
            :key="branch.nodeId" 
            :index="idx" 
            :node="branch"
            :parentLength="node.conditionNodes.length" 
            :add-node="addNode" :delete-node="deleteNode"
            @delbranch="delBranch(node, $event)"
          />
        </div>

        <add-node-btn :node="node" :add-node="addNode" :delete-node="deleteNode" />
      </div>
    </div>
    <node-wrap-box v-else :node="node" :delete-node="deleteNode" />
    <add-node-btn v-if="node.type !== 'branch'" :node="node" :add-node="addNode" :delete-node="deleteNode" />
    <node-main v-if="node.childNode" :node="node.childNode"  :add-node="addNode" :delete-node="deleteNode" />
  </div>
</template>
<script>
import addNodeBtn from './add-node-btn';
import colBox from './col-box';
import nodeWrapBox from './node-wrap-box';
const getRandom = () => {
  return Math.floor((Math.random() + Math.floor(Math.random() * 9 + 1)) * Math.pow(10, 9));
}
export default {
  name: 'node-main',
  components: {
    'add-node-btn': addNodeBtn,
    'col-box': colBox,
    'node-wrap-box': nodeWrapBox
  },
  props: {
    node: {
      type: Object,
      default: () => null
    },
    addNode: {
      type: Function,
      default: () => null
    },
    deleteNode: {
      type: Function,
      default: () => null
    }
  },
  created() {

  },
  methods: {
    addBranch(childNode) {
      childNode.conditionNodes.push({
        "name": `条件${childNode.conditionNodes.length + 1}`,
        "type": "condition",
        "prevId": childNode.nodeId,
        "nodeId": getRandom()
      })
    },
    delBranch(node, idx) {
      if (node.conditionNodes.length <= 2) {
        this.deleteNode(node.nodeId);
      } else {
        node.conditionNodes.splice(idx, 1);
      }
    },
  }
}
</script>