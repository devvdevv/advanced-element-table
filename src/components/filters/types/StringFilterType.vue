<template>
  <div>
    <el-input
      placeholder="Please input"
      v-model="input"
      @change="onChange"
    ></el-input>
  </div>
</template>

<script>
export default {
  data() {
    return {
      input: "",
    };
  },
  methods: {
    onChange() {},
  },
};
</script>