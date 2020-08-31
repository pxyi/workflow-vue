<template>
  <div class="col-box">
    
    <div class="top-left-cover-line" v-if="index === 0"></div>
    <div class="bottom-left-cover-line" v-if="index === 0"></div>
    <div class="condition-node">
      <div class="condition-node-box">
        <div class="auto-judge">
          <div class="title-wrapper">
            <span class="editable-title">
              <input type="text" v-model="node.name">
            </span>
            <span class="priority-title">优先级{{ index + 1 }}</span>
            <span class="svg-icon close" @click="$emit('delbranch', index)">删除</span>
          </div>
          <div class="content-wrapper">
            <div class="content">请设置条件</div>
          </div>
        </div>
        <add-node-btn :node="node" :add-node="addNode" :delete-node="deleteNode" />
      </div>
    </div>
    <node-main v-if="node && node.childNode" :node="node.childNode" :add-node="addNode" :delete-node="deleteNode" />
    <div class="top-right-cover-line" v-if="index === parentLength - 1"></div>
    <div class="bottom-right-cover-line" v-if="index === parentLength - 1"></div>
  </div>
</template>
<script>
import addNodeBtn from './add-node-btn';
export default {
  name: 'col-box',
  components: {
    'add-node-btn': addNodeBtn,
    'node-main': () => {
      return import('./node-main');
    }
  },
  created() {
    
  },
  props: {
    node: {
      type: Object,
      default: () => null
    },
    index: {
      type: Number
    },
    parentLength: {
      type: Number
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
  methods: {
  }
}
</script>