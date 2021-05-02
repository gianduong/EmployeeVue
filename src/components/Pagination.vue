<template>
  <div v-if="totalPage > 1 && page <= totalPage && page >= 1" class="pagination">
    <router-link to="/employee?page=1" v-if="page > 1" class="pagination-item">
      <div class="first-page"></div>
    </router-link>
    <router-link
      :to="'/employee?page=' + (page - 1)"
      v-if="page > 1"
      class="pagination-item"
    >
      <div class="prev-page"></div>
    </router-link>

    <router-link
      v-for="n in rangePage"
      :key="n"
      class="pagination-item"
      :to="'/employee?page=' + n"
    >
      <div class="page" :class="{ active: n == page }">{{ n }}</div>
    </router-link>

    <router-link
      :to="'/employee?page=' + (page + 1)"
      v-if="page < totalPage"
      class="pagination-item"
    >
      <div class="next-page"></div>
    </router-link>
    <router-link
      :to="'/employee?page=' + totalPage"
      v-if="page < totalPage"
      class="pagination-item"
    >
      <div class="last-page"></div>
    </router-link>
  </div>
</template>

<script>
export default {
  name: "Pagination",
  props: {
    /**
     * Trang hiện tại.
     */
    page: {
      type: Number,
      default: 1,
    },

    /**
     * Tổng số trang.
     */
    totalPage: {
      type: Number,
      default: 1,
    },
  },

  computed: {
    rangePage: function () {
      let range = [];
      let start = this.page > 3 ? this.page - 2 : 1;
      let end = this.page < this.totalPage - 3 ? this.page + 2 : this.totalPage;
      for (let i = start; i <= end; i++) range.push(i);
      return range;
    },
  },
};
</script>

<style scoped>
.pagination {
  display: flex;
  align-items: center;
}

.pagination .pagination-item {
  display: inline-block;
  height: 24px;
  width: 24px;
  line-height: 24px;
  cursor: pointer;
  margin-left: 4px;
  margin-right: 4px;
  text-decoration: none;
  color: #000;
}

.pagination .pagination-item .page {
  text-align: center;
  border-radius: 50%;
  border: 1px solid #bbb;
}

.pagination .pagination-item .page.active {
  background-color: #019160;
  color: #fff;
}

.pagination .pagination-item .page:hover:not(.active) {
  background-color: #e5e5e5;
}

.pagination .first-page {
  background-image: url("../assets/icon/btn-firstpage.svg");
  background-position: center;
  background-repeat: no-repeat;
  background-size: inherit;
  height: 100%;
  width: 100%;
}

.pagination .prev-page {
  background-image: url("../assets/icon/btn-prev-page.svg");
  background-position: center;
  background-repeat: no-repeat;
  background-size: inherit;
  height: 100%;
  width: 100%;
}

.pagination .next-page {
  background-image: url("../assets/icon/btn-next-page.svg");
  background-position: center;
  background-repeat: no-repeat;
  background-size: inherit;
  height: 100%;
  width: 100%;
}

.pagination .last-page {
  background-image: url("../assets/icon/btn-lastpage.svg");
  background-position: center;
  background-repeat: no-repeat;
  background-size: inherit;
  height: 100%;
  width: 100%;
}
</style>