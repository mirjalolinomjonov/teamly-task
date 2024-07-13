<template>
  <div class="selection-wrapper">
    <span
      v-if="node.checked && (!node.children || node.children.length == 0)"
      class="selection-wrapper__item"
    >
      {{ node.label }}
      <icon-base name="cancel" @click="cancelNode(node)" />
    </span>

    <template v-for="(item, index) in node.children">
      <nodes
        v-if="node.children && node.children.length"
        :key="index"
        :node="item"
        @cancelNode="cancelNode"
      />
    </template>
  </div>
</template>

<script>
import IconBase from '@/components/common/IconBase.vue'
export default {
  name: 'ResultNode',
  components: {
    IconBase
  },
  props: {
    node: {
      type: Object,
      requared: () => true
    }
  },
  methods: {
    cancelNode(node) {
      this.$emit('cancel-node', node, false)
    }
  }
}
</script>

<style lang="scss" scoped>
.selection-wrapper {
  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;
  gap: 16px;
  // selection-wrapper__item
  &__item {
    display: inline-flex;
    align-items: center;
    gap: 4px;
    height: 42px;
    padding: 10px 10px 10px 12px;
    border-right: 1px solid #d8dde5;
    border-top: 1px solid #d8dde5;
    border-radius: 24px;
    background: #fff;
    color: #2e3540;
    font-size: 14px;
    font-weight: 400;
    line-height: 22px;
    letter-spacing: 2%;
    .icon {
      cursor: pointer;
    }
  }
}
</style>
