<template>
  <div class="node-wrap-box">
    <div>
      <div class="title" :class="[node.type]">{{ node.name }}<span @click="deleteNode(node.nodeId)">删除</span></div>
      <div class="content" @click="handle">所有人 ></div>
    </div>

    <a-drawer
      :title="node.name"
      :width="520"
      :visible="showDrawer"
      :maskClosable="true"
      @close="() => { showDrawer = false}"
    >
      <a-form-model ref="ruleForm" :model="ruleForm" :labelCol="{span: 4}" :wrapperCol="{ span: 20 }">
        <a-form-model-item ref="name" :label="node.type ==='approver' ? '审批人' : '抄送人'" prop="xx">
          <a-radio-group>
            <a-radio-button :value="1">指定成员</a-radio-button>
            <a-radio-button :value="2" v-if="node.type !== 'notifier'">指定岗位</a-radio-button>
            <a-radio-button :value="3">发起人主管</a-radio-button>
          </a-radio-group>
        </a-form-model-item>
        <a-form-model-item ref="name" label="审批方式" prop="name">
          <a-radio-group>
            <a-radio-button value="a">会审</a-radio-button>
            <a-radio-button value="b">或审</a-radio-button>
          </a-radio-group>
        </a-form-model-item>
      </a-form-model>
    </a-drawer>
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
  data() {
    return {
      showDrawer: false,
      ruleForm: {

      }
    }
  },
  created() {

  },
  methods: {
    handle() {
      console.log(this.node)
      this.showDrawer = true;
      this.ruleForm = {
        
      }
    } 
  }
}
</script>