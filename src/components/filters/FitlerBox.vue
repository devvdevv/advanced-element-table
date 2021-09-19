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
      <div class="apply-btn">
        <el-button class="apply" type="primary" size="medium">Apply</el-button>
      </div>
    </div>
  </el-popover>
</template>

<style lang="css">
.apply-btn {
  width: 180px;
  margin: 10px auto 0 auto;
}

.apply {
  width: 180px;
}
</style>

<script>
import AttributesFilterType from "./types/AttributesFilterType.vue";
import StringFilterType from "./types/StringFilterType.vue";
export default {
  components: { StringFilterType, AttributesFilterType },
  props: ["filterType"],
  data() {
    return {
      visible: false,
    };
  },
};
</script>