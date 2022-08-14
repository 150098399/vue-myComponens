<template>
  <div class="tree2">
    <el-button type="primary" @click="save"> 保存 </el-button>
    <div class="question_info_lists">
      <el-tree
        ref="tree"
        :data="treeData"
        highlight-current
        :expand-on-click-node="false"
      >
        <span class="custom-tree-node" slot-scope="{ node, data }">
          <span v-show="!data.isEdit">{{ node.label }}</span>
          <input
            v-model="data.label"
            ref="mask"
            @blur="changeInput(node, data)"
            v-show="data.isEdit"
          />
          <div class="tree_node_icon" style="float: right">
            <i
              v-if="node.level <= 4"
              class="el-icon-plus"
              @click="append(node, data)"
            ></i>
            <i class="el-icon-edit" @click="nodeEdit(data, $event)"></i>
            <i class="el-icon-delete" @click="nodeDelete(node, data)"></i>
          </div>
        </span>
      </el-tree>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      treeData: [
        {
          id: 0,
          label: "军团",
          isEdit: false,
          children: [
            {
              id: 1,
              label: "一级 1",
              isEdit: false,
              children: [
                {
                  id: 4,
                  label: "二级 1-1",
                  isEdit: false,
                  children: [
                    { id: 9, label: "三级 1-1-1", isEdit: false, children: [] },
                    {
                      id: 10,
                      label: "三级 1-1-2",
                      isEdit: false,
                      children: [],
                    },
                    {
                      id: 11,
                      label: "三级 1-1-3",
                      isEdit: false,
                      children: [],
                    },
                  ],
                },

                {
                  id: 12,
                  label: "二级 1-2",
                  isEdit: false,
                  children: [],
                },

                {
                  id: 13,
                  label: "二级 1-3",
                  isEdit: false,
                  children: [],
                },
              ],
            },

            {
              id: 2,
              label: "一级 2",
              isEdit: false,
              children: [
                { id: 5, label: "二级 2-1", isEdit: false, children: [] },
                {
                  id: 6,
                  label: "二级 2-2",
                  isEdit: false,
                  children: [],
                },
              ],
            },

            {
              id: 3,
              label: "一级 3",
              isEdit: false,
              children: [
                { id: 7, label: "二级 3-1", isEdit: false, children: [] },
                {
                  id: 8,
                  label: "二级 3-2",
                  isEdit: false,
                  children: [],
                },
              ],
            },
          ],
        },
      ],
    };
  },

  methods: {
    // 保存 获取整颗树
    save() {
      console.log(this.$refs.tree.data);
    },
    // input失焦后改变isEdit值
    changeInput(node, data) {
      data.isEdit = false;
      if (!data.label) {
        this.nodeDelete(node, data);
      }
    },
    // 增加节点
    append(node, data) {
      var maxid = "10009";
      //新增数据
      const nodeapp = {
        id: ++maxid,
        label: "",
        isEdit: true,
        children: [],
      };
      data.children.push(nodeapp);
      if (!node.expanded) {
        node.expanded = true;
      }

      setTimeout(() => {
        if (this.$refs.mask) {
          this.$refs.mask.focus();
        }
      }, 0);
    },

    // 编辑
    nodeEdit(data, ev) {
      data.isEdit = true;
      this.$nextTick(() => {
        const $input =
          ev.target.parentNode.parentNode.querySelector("input") ||
          ev.target.parentElement.parentElement.querySelector("input");

        !$input ? "" : $input.focus();
      });
    },

    // 节点删除
    nodeDelete(node, data) {
      const parent = node.parent;
      const children = parent.data.children || parent.data;
      const index = children.findIndex((d) => d.id === data.id);
      children.splice(index, 1);
    },
  },
};
</script>

<style scoped></style>
