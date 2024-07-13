<template>
  <li class="list__item">
    <span class="costum-checkbox">
      <label>
        <input
          class="costum-checkbox__input"
          type="checkbox"
          :checked="node.checked"
          :indeterminate="node.indeterminate"
          @change="toggleCheck"
        />
        <!-- costum checkbox -->
        <span class="costum-checkbox__icon" :class="{ indeterminate: node.indeterminate }"></span>
      </label>
      <span
        @click="isDropdown = !isDropdown"
        :class="{ nested: !node.children || node.children.length === 0 }"
      >
        {{ node.label }}
        <icon-base
          v-if="node.children && node.children.length"
          class="dropdown-icon"
          :class="{ active: isDropdown }"
          name="dropdown"
        />
      </span>
    </span>

    <ul
      class="nested-list"
      :class="{ open: isDropdown }"
      v-if="node.children && node.children.length"
    >
      <TreeNode v-for="child in node.children" :key="child.id" :node="child" @check="handleCheck" />
    </ul>
  </li>
</template>

<script>
import IconBase from './common/IconBase.vue'
export default {
  components: { IconBase },
  name: 'TreeNode',
  props: {
    node: {
      type: Object,
      required: () => true
    }
  },
  data() {
    return {
      isDropdown: false
    }
  },

  methods: {
    toggleCheck(event) {
      const checked = event.target.checked
      this.$emit('check', this.node, checked)
    },
    handleCheck(node, checked) {
      this.$emit('check', node, checked)
    }
  }
}
</script>

<style lang="scss" scoped>
.list__item:not(:last-child) {
  margin-bottom: 8px;
}
.costum-checkbox {
  color: #2e3540;
  font-size: 14px;
  font-weight: 400;
  line-height: 22px;
  letter-spacing: 2%;
  // margin-bottom: 8px;
  display: flex;
  align-items: center;

  // costum-checkbox__icon
  &__icon {
    position: relative;
    display: inline-block;
    width: 20px;
    height: 20px;
    margin-right: 10px;
    border: 1px solid #d9d9d9;
    border-radius: 4px;
    transition: all linear 0.25s;
    cursor: pointer;
    &::before {
      content: '';
      width: 5px;
      height: 10px;
      border-bottom: 2px solid #fff;
      border-right: 2px solid #fff;
      position: absolute;
      top: 40%;
      left: 50%;
      transform: translate(-50%, -50%) rotate(45deg);
      visibility: hidden;
    }
    &.indeterminate {
      background: #f52053;
      border-color: #f52053;
      &::after {
        content: '';
        height: 1.5px;
        width: 10px;
        background: #fff;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
      }
    }
    &:hover {
      border-color: #f52053;
    }
  }

  label,
  label + span,
  label + span .dropdown-icon {
    line-height: 0;
  }
  label + span {
    display: inline-flex;
    align-items: center;
    cursor: pointer;
    user-select: none;
    color: #2e3540;
    font-size: 14px;
    font-weight: 600;
    line-height: 20px;
    letter-spacing: 1%;
    &.nested {
      font-weight: 400;
      line-height: 22px;
      letter-spacing: 2%;
    }
  }

  span .dropdown-icon {
    display: inline-block;
    margin-left: 4px;
    transition: all 0.25s linear;
    &.active {
      transform: rotateX(180deg);
    }
  }
  // costum-checkbox__input
  &__input {
    position: absolute;
    display: none;
  }
  &__input:checked + .costum-checkbox__icon {
    border-color: #f52053;
    background: #f52053;
  }
  &__input:checked + .costum-checkbox__icon::before {
    visibility: visible;
  }
}

.nested-list {
  max-height: 0;
  overflow: hidden;
  margin: 8px 0 0 24px;
  transition: max-height 0.25s linear;
  &.open {
    transition: all 0.25s linear;
    max-height: max-content;
  }
}
</style>
