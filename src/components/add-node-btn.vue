<template>
  <div class="add-node-btn-box">
    <div class="add-node-btn">
      <a-popover trigger="click">
        <template slot="content">
          <div class="popover-inner-content">
            <div @click="add('approver')">审批人</div>
            <div @click="add('notifier')">抄送人</div>
            <div @click="add('branch')">条件分支</div>
          </div>
        </template>
        <button class="btn"><span>+</span></button>
      </a-popover>
    </div>
  </div>
</template>
<script>
export default {
  name: 'add-node-btn',
  props: {
    node: {
      type: Object,
      default: () => {}
    },
    addNode: {
      type: Function,
      default: () => null
    }
  },
  created() {
    
  },
  methods: {
    add(type) {
      this.addNode({ nodeId: this.node.nodeId, type });
    }
  }
}
</script>