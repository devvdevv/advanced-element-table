<template>
  <el-popover
    placement="bottom"
    title="Filter (developing, just 4 view)"
    width="200"
    trigger="manual"
    v-model="visible"
  >
    <button slot="reference" @click="visible = !visible">
      <i slot="reference" class="el-icon-s-operation"></i>
    </button>
    <div class="content">
      <string-filter-type v-if="filterType === 'string'"></string-filter-type>
      <attributes-filter-type
        v-else-if="filterType === 'attribute'"
      ></attributes-filter-type>
      <div class="btns">
        <div class="btn">
          <el-button class="apply btn" type="primary" size="medium" @click="apply()">Apply</el-button>
        </div>
        <div class="btn" v-if="column.filtering && column.filtering === true">
          <el-button class="reset btn" @click="reset()" size="medium" plain>Reset</el-button>
        </div>
      </div>
    </div>
  </el-popover>
</template>

<style lang="css">
.btns {
  width: 180px;
  margin: 10px auto 0 auto;
}

.btn {
  width: 180px;
  margin-top: 4px;
}
</style>

<script>
import AttributesFilterType from "./types/AttributesFilterType.vue";
import StringFilterType from "./types/StringFilterType.vue";
export default {
  components: { StringFilterType, AttributesFilterType },
  props: ["filterType", "columnName", "column"],
  data() {
    return {
      visible: false,
    };
  },
  methods: {
    apply() {
      this.$emit("onFiltering", this.columnName); 
    },
    reset() {
      this.$emit("onCancel", this.columnName); 
    }
  }
};
</script>