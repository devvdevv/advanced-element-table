Use Vue Data Tables to build Generic Table.
https://github.com/njleonzhang/vue-data-tables
https://element.eleme.io/#/en-US/component/table

%================================================
%
% author: Pham Bao Long | devvdevv
%
%================================================

<template>
  <div>
    <el-table
      v-loading="isDataLoading"
      element-loading-text="Loading..."
      element-loading-spinner="el-icon-loading"
      element-loading-background="rgba(0, 0, 0, 0.8)"
      :data="data"
      :border="d_border"
      style="width: 100%"
      v-if="show"
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
          <div style="display:flex">
            <div
              v-if="(typeof columns[column].header === 'object')"
              :is="columns[column].header"
            >
            </div>
            <div
              v-else-if="(typeof columns[column].header === 'function')"
              :formatter="columns[column].header"
            >
            </div>
            <div v-else> {{ columns[column].label }}</div>
            <div class="applied-tools">
              <div class="filtered" v-if="columns[column].filtering">
                <img src="../assets/filter.png" alt="filtered" width="10">
              </div>
            </div>
            <div class="right-side">
              <div
                class="filter"
                v-if="columns[column].filter && columns[column].filter === true">
                <filter-box
                  :filterType="columns[column].filterType"
                  :column="columns[column]"
                  :columnName="column"
                  @onFiltering="onApplyFilter"
                  @onCancel="onCancelFilter"
                  ></filter-box>
              </div>
            </div>
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

<style lang="css">
.applied-tools {
  padding-left: 3px;
}
.right-side {
  margin-left: auto;
}
</style>

<script>
import FilterBox from './filters/FilterBox.vue';

export default {
  components: { FilterBox },
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
      show: true,
      d_border: this.border ? this.border : false,
      d_background: this.p_background ? this.p_background : false,
      d_small: this.p_small ? this.p_small : false,
      d_pageSize: this.pageSize ? this.pageSize : 10,
      d_rows: this.rows,
      backupRows: this.rows,
    };
  },
  watch: {
    rows: {
      handler(newVal) {
        this.d_rows = newVal;
        this.backupRows = newVal;
      }
    }
  },
  computed: {
    isDataLoading() {
      return this.isLoading;
    },
    data: {
      get: function() {
        return this.d_rows;
      },
      set: function(data) {
        this.d_rows = data;
      }
    },
  },
  methods: {
    onPageChange(e) {
      this.$emit("onPageChange", e);
    },
    onApplyFilter(column, value) {
      this.columns[column].filtering = true;
      this.filter(column, value);
      this.forceRefresh();
    },
    onCancelFilter(column) {
      this.columns[column].filtering = false;
      this.unfilter();
      this.forceRefresh();
    },
    // TODO: find better solution.
    // hack here to render filter icon on header => NOT GOOD
    forceRefresh() {
      this.show = !this.show;
      this.$nextTick(() => (this.show = true));
    },
    filter(property, val) {
      this.backupRows = this.rows;
      let filteredData = [];
      for (let row of this.backupRows) {
        if (row[property] === val) {
          filteredData.push(row);
        }
      }
      this.data = filteredData;
    },
    unfilter() {
      this.data = this.backupRows;
    }
  },
};
</script>

