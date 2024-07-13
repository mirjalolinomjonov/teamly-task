<template>
  <main class="container">
    <!-- ASIDE -->
    <aside class="aside">
      <!-- search input -->
      <label class="aside__search" for="search">
        <input
          type="text"
          class="aside__search-input"
          id="search"
          placeholder="Type to choose"
          autocomplete="off"
          v-model="searchNode"
        />
        <icon-base class="aside__search-magnifier" name="magnifier" />
      </label>

      <!-- tree list -->
      <ul class="list">
        <TreeNode
          v-for="node in searchFilter"
          :key="node.id"
          :node="node"
          @check="handleCheckChange"
        />
      </ul>
    </aside>

    <!-- RESULT SECTION -->
    <section class="result-section">
      <Nodes v-for="(item, index) in treeData" :key="index" :node="item" @cancelNode="cancelNode" />
    </section>
  </main>
</template>

<script>
import TreeNode from '@/components/TreeNode.vue'
import Nodes from '@/components/Nodes.vue'
import IconBase from '@/components/common/IconBase.vue'
export default {
  name: 'indexLayout',
  components: { TreeNode, Nodes, IconBase },
  data() {
    return {
      searchNode: '',
      treeData: [
        {
          id: 1,
          label: 'Food & Drinks',
          checked: false,
          indeterminate: false,
          parent: null,

          children: [
            {
              id: 2,
              label: 'Plov',
              checked: false,
              indeterminate: false,
              parent: null,
              children: []
            },
            {
              id: 3,
              label: 'Soup',
              checked: false,
              indeterminate: false,
              parent: null,
              children: []
            },
            {
              id: 4,
              label: 'Juice',
              checked: false,
              indeterminate: false,
              parent: null,
              children: [
                { id: 5, label: 'Dena', checked: false, indeterminate: false, parent: null },
                { id: 51, label: 'Dinay', checked: false, indeterminate: false, parent: null },
                { id: 52, label: 'Bliss', checked: false, indeterminate: false, parent: null },
                { id: 53, label: 'Dolce', checked: false, indeterminate: false, parent: null },
                { id: 54, label: 'Rich', checked: false, indeterminate: false, parent: null }
              ]
            }
          ]
        },
        {
          id: 6,
          label: 'Nature',
          checked: false,
          indeterminate: false,
          parent: null,
          children: [
            {
              id: 7,
              label: 'Beaches',
              checked: false,
              indeterminate: false,
              parent: null,
              children: []
            },
            {
              id: 8,
              label: 'Parks',
              checked: false,
              indeterminate: false,
              parent: null,
              children: []
            },
            {
              id: 81,
              label: 'Aquarium & Oceanarium',
              checked: false,
              indeterminate: false,
              parent: null,
              children: []
            },
            {
              id: 82,
              label: 'Zoos & Sanctuaries',
              checked: false,
              indeterminate: false,
              parent: null,
              children: []
            },
            {
              id: 83,
              label: 'Mountains',
              checked: false,
              indeterminate: false,
              parent: null,
              children: []
            }
          ]
        },
        {
          id: 9,
          label: 'Music, Dance, Cinema',
          checked: false,
          indeterminate: false,
          parent: null,
          children: []
        }
      ],
      checkedItems: []
    }
  },
  computed: {
    searchFilter() {
      return this.treeData.filter((node) =>
        node.label.toLowerCase().includes(this.searchNode.toLowerCase())
      )
    }
  },
  created() {
    this.setParentReferences(this.treeData, null)
  },
  methods: {
    setParentReferences(nodes, parent) {
      nodes.forEach((node) => {
        node.parent = parent
        if (node.children) {
          this.setParentReferences(node.children, node)
        }
      })
    },
    handleCheckChange(node, checked) {
      node.checked = checked
      node.indeterminate = false
      this.updateChildNodes(node, checked)
      this.updateParentNodes(node)
    },
    updateChildNodes(node, checked) {
      if (node.children && node.children.length) {
        node.children.forEach((child) => {
          child.checked = checked
          child.indeterminate = false
          this.updateChildNodes(child, checked)
        })
      }
    },
    updateParentNodes(node) {
      if (!node.parent) return
      const parent = node.parent
      const allChecked = parent.children.every((child) => child.checked)
      const allUnchecked = parent.children.every((child) => !child.checked)
      parent.checked = allChecked
      parent.indeterminate = !allChecked && !allUnchecked
      this.updateParentNodes(parent)
    },
    cancelNode(node, checked) {
      console.log(node, checked)
      this.handleCheckChange(node, checked)
    },
    collectCheckedItems() {
      this.checkedItems = []
      this.collectCheckedChildItems(this.treeData)
    },
    collectCheckedChildItems(nodes) {
      nodes.forEach((node) => {
        if (node.checked && (!node.children || node.children.length === 0)) {
          this.checkedItems.push(node)
        }
        if (node.children) {
          this.collectCheckedChildItems(node.children)
        }
      })
    }
  }
}
</script>

<style lang="scss">
.container {
  display: flex;
  height: 100vh;
}

// ASIDE
.aside {
  width: 319px;
  height: 100%;
  background: #fff;
  color: rgb(46, 53, 64);
  // aside__search
  &__search {
    // position: relative;
    display: flex;
    align-items: center;
    gap: 8px;
    width: 280px;
    margin: 10px 26px 13px 10px;
    padding: 12px 16px;
    border-radius: 10px;
    border: 1px solid rgb(217, 217, 217);
    // aside__search-input
    &-input {
      color: rgb(121, 124, 129);
      font-size: 14px;
      font-weight: 400;
      line-height: 22px;
      letter-spacing: 0%;
      border: none;
      width: 100%;
      height: 100%;
      &::placeholder {
        color: rgb(121, 124, 129);
        font-size: 14px;
        font-weight: 400;
        line-height: 22px;
        letter-spacing: 0%;
        text-align: left;
      }
    }

    // aside__search-magnifier
    &-magnifire {
      // position: absolute;
      // right: 16px;
      // top: 50%;
      // transform: translateY(-50%);
      flex-shrink: 0;
    }
  }
}
.list {
  padding: 0 20px 20px;
}

// SECTION
.result-section {
  background: #f9f9f9;
  width: calc(100vw - 319px);
  padding: 32px 46px;
  display: flex;
  align-items: flex-start;
  gap: 16px;
}
</style>
