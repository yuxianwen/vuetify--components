<template>
  <div class="d-flex align-center mt-4 justify-end">
    共 {{ $attrs.total }} 条
    <v-card class="ml-5" width="120" elevation="0">
      <v-select
        v-model="size"
        :items="pageSizes"
        outlined
        dense
        menu-props="auto"
        @change="pageSizeChange"
        hide-details
      >
        <template v-slot:selection="{ item }"> {{ item }}条/页 </template>
        <template v-slot:item="{ item }"> {{ item }}条/页 </template>
      </v-select>
    </v-card>
    <v-pagination
      class="pagination ml-3"
      v-model="currentPage"
      @input="pageCurrentChange"
      v-bind="$attrs"
    ></v-pagination>
  </div>
</template>
<script>
export default {
  props: {
    pageSize: { default: 10, type: Number },
    value: { default: 1, type: Number }
  },

  // watch
  watch: {
    value() {
      this.currentPage = this.value;
    },
    currentPage(val) {
      this.$emit("input", val);
    }
  },
  data() {
    return {
      pageSizes: [10, 20, 30, 40, 50],
      currentPage: this.value,
      size: this.pageSize
    };
  },
  methods: {
    // 改变每页数量
    pageSizeChange(val) {
      this.$emit("size-change", val);
    },
    // 改变页数
    pageCurrentChange(val) {
      this.$nextTick(() => {
        this.$emit("current-change", val);
      });
    }
  }
};
</script>

<style lang="scss">
.pagination {
  .v-pagination {
    justify-content: flex-end;
  }
}
</style>
