<template>
  <div>
    <img :src="'https://www.countryflags.io/' + row[column] + '/flat/64.png'" width="30" />
  </div>
</template>

<script>
export default {
  name: "FlagRenderer",
  props: ["row", "column"],
};
</script>