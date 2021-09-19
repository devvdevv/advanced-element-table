<template>
  <div>
    <el-select v-model="value" @change="onChange" filterable placeholder="Select">
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value"
      >
      </el-option>
    </el-select>
  </div>
</template>

<script>
export default {
  data() {
    return {
      options: [
        {
          value: "M",
          label: "M",
        },
        {
          value: "F",
          label: "F",
        },
      ],
      value: "",
    };
  },
  methods: {
    onChange(val) {
      this.$emit("onChange", val);
    }
  }
};
</script>