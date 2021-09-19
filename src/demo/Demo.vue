<template>
  <div style="width: 800px; margin: auto">
    <el-card class="box-card">
      <advanced-table
        :isLoading="isLoading"
        :border="false"
        :rows="rows"
        :columns="columns"
        :total="total"
        :current="pageNum"
        :pageSize="pageSize"
        :p_background="true"
        @onPageChange="onPageChange"
      ></advanced-table>
    </el-card>
  </div>
</template>

<script>
import AdvancedTable from "../components/AdvancedTable.vue";
import { DATA } from "./data";
import BronzeHeaderVue from './headerRenderers/BronzeHeader.vue';
import GoldHeaderVue from './headerRenderers/GoldHeader.vue';
import SilverHeaderVue from './headerRenderers/SilverHeader.vue';
import FlagRendererVue from "./renderers/FlagRenderer.vue";
export default {
  components: { AdvancedTable },
  mounted() {
    console.log(
      "%cWOOHO~ If you see this I guess you are developer, thanks to eye on this project :D",
      "font-weight: bold;"
    );
    console.table(JSON.parse(JSON.stringify(DATA)));
    this.isLoading = true;
    setInterval(() => {
      this.isLoading = false;
    }, 999);
  },
  data() {
    return {
      rows: DATA,
      columns: {
        athlete_name: {
          id: 2,
          label: "Athlete",
          minWidth: 40,
          filter: true,
          filterType: 'string'
        },
        gender: {
          id: 3,
          label: "Gender",
          minWidth: 15,
          filter: true,
          filterType: 'attribute'
        },
        country: {
          id: 4,
          label: "Country",
          minWidth: 12,
          formatter: FlagRendererVue,
        },
        gold_medals: {
          id: 5,
          label: "Gold",
          minWidth: 10,
          header: GoldHeaderVue,
        },
        silver_medals: {
          id: 6,
          label: "Silver",
          minWidth: 10,
          header: SilverHeaderVue,
        },
        bronze_medals: {
          id: 7,
          label: "Bronze",
          minWidth: 10,
          header: BronzeHeaderVue
        },
      },
      total: 20,
      isLoading: false,
      pageNum: 1,
      pageSize: 20,
    };
  },
  methods: {
    onPageChange(e) {
      this.pageNum = e;
    },
  },
};
</script>