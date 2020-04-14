<template>
  <div class="diyTable">
    <el-table
      :data="data"
      ref="multipleTable"
      :style="`width:${options.width};text-align:${options.align}`"
      :height="options.height"
      :max-height="options.maxHeight"
      :border="options.border"
      :size="options.size"
      :stripe="optiones.stripe"
      @selection-change="handleSelectionChange"
      v-loading="loading"
    >
      <el-table-column
        type="selection"
        style="width: 55px;"
        v-if="options && options.selection && (!options.isShow || options.isShow()) "
      />
      <el-table-column
        v-for="(item,i) in options.column"
        :label="item.label"
        :key="i"
        :prop="item.prop"
        :width="item.width"
      >
        <template slot-scope="scope">
          <slot :name="item.prop" v-if="item.slot" :row="scope.row"></slot>
          <p v-else>{{scope.row[item.prop]}}</p>
        </template>
      </el-table-column>
      <el-table-column label="操作" v-if="options.menu">
        <template slot-scope="scope">
          <slot name="menu" :row="scope.row"></slot>
        </template>
      </el-table-column>
    </el-table>
    <div class="pagination">
      <el-pagination
        v-if="options.paging"
        @size-change="handleSizeChange"
        @current-change="handleSizeChange"
        :current-page.sync="pages.currentPage"
        :page-size="pages.pageSize"
        layout="total, prev, pager, next, jumper"
        :total="pages.total"
      ></el-pagination>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    options: {
      default: () => {}
    },
    data: {
      default: () => {}
    },
    pages: {
      default: () => {}
    },
    loading: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {};
  },
  methods: {
    // 表格翻页
    handleSizeChange(page) {
      this.pages.page = page;
      this.$emit("on-load", this.pages);
    },
    handleSelectionChange(val) {
      this.$emit("handleSelectionChange", val);
    }
  },
  created() {
    this.handleSizeChange();
  }
};
</script>

<style lang="scss" scoped>
.diyTable {
  .pagination {
    display: flex;
    justify-content: flex-end;
  }
}
</style>