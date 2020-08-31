<template>
  <div class="node-wrap-box">
    <div>
      <div class="title" :class="[node.type]">{{ node.name }}<span @click="deleteNode(node.nodeId)">删除</span></div>
      <div class="content">所有人 ></div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    node: {
      type: Object,
      default: () => null
    },
    deleteNode: {
      type: Function,
      default: () => null
    }
  },
  created() {
    console.log(this.node)
  },
  methods: {
    
  }
}
</script>