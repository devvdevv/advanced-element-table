Use Vue Data Tables to build Generic Table.
https://github.com/njleonzhang/vue-data-tables
https://element.eleme.io/#/en-US/component/table

%================================================
%
% author: Pham Bao Long
%
%================================================

<template>
  <div>
    <el-table
      v-loading="isDataLoading"
      element-loading-text="Loading..."
      element-loading-spinner="el-icon-loading"
      element-loading-background="rgba(0, 0, 0, 0.8)"
      :data="rows"
      :border="d_border"
      style="width: 100%"
    >
      <el-table-column
        v-for="(i, column) of columns"
        :key="i.id"
        :prop="column"
        :label="columns[column] ? columns[column].label : column"
        :min-width="columns[column].minWidth ? columns[column].minWidth : null"
        :width="columns[column].width ? columns[column].width : 'auto'"
        :formatter="
          typeof columns[column].formatter === 'function'
            ? columns[column].formatter
            : null
        "
        :type="columns[column].type ? columns[column].type : null"
      >
        <template
          slot="header"
          v-if="columns[column].header !== null"
        >
          <div
            v-if="(typeof columns[column].formatter !== 'function')"
            :is="columns[column].header"
          >
          </div>
          <div
            v-else
            :formatter="columns[column].header"
          >
          </div>
        </template>
        <template
          #default="{ row }"
          v-if="(typeof columns[column].formatter !== 'function')">
          <div
            v-if="columns[column].formatter"
            :is="columns[column].formatter"
            :row="row"
            :column="column"
          ></div>
          <template v-else>{{ row[column] }}</template>
        </template>
      </el-table-column>
    </el-table>
    <br />
    <el-pagination
      style="float: right"
      layout="prev, pager, next"
      :background="d_background"
      :small="d_small"
      :total="total"
      :page-size="d_pageSize"
      :current-page="current"
      @current-change="onPageChange"
    >
    </el-pagination>
  </div>
</template>

  <script>
export default {
  props: [
    "rows",
    "columns",
    "border",
    "total",
    "current",
    "pageSize",
    "p_background",
    "p_small",
    "isLoading",
  ],
  mounted() {},
  data() {
    return {
      d_border: this.border ? this.border : false,
      d_background: this.p_background ? this.p_background : false,
      d_small: this.p_small ? this.p_small : false,
      d_pageSize: this.pageSize ? this.pageSize : 10,
    };
  },
  computed: {
    isDataLoading() {
      return this.isLoading;
    },
  },
  methods: {
    onPageChange(e) {
      this.$emit("onPageChange", e);
    },
  },
};
</script>

