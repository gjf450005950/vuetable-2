<template>
  <form class="form-inline text-right">
    <div v-show="tablePagination && lastPage > firstPage" class="form-group">
      <ul class="pagination">
        <li :class="['btn-nav', $_css.linkClass, isOnFirstPage ? $_css.disabledClass : '']">
          <a @click="loadPage(firstPage)">
            <i v-if="$_css.icons.first != ''" :class="[$_css.icons.first]"></i>
            <span v-else>&laquo;</span>
          </a>
        </li>
        <li :class="['btn-nav', $_css.linkClass, isOnFirstPage ? $_css.disabledClass : '']">
          <a @click="loadPage('prev')">
            <i v-if="$_css.icons.next != ''" :class="[$_css.icons.prev]"></i>
            <span v-else>&nbsp;&lsaquo;</span>
          </a>
        </li>
        <template v-if="notEnoughPages">
          <template v-for="(n, i) in totalPage">
            <li :class="[$_css.pageClass, isCurrentPage(i+firstPage) ? $_css.activeClass : '']">
              <a @click="loadPage(i+firstPage)" :key="i" v-html="n"></a>
            </li>
          </template>
        </template>
        <template v-else>
          <template v-for="(n, i) in windowSize">
            <li
              :class="[$_css.pageClass, isCurrentPage(windowStart+i+firstPage-1) ? $_css.activeClass : '']"
            >
              <a @click="loadPage(windowStart+i+firstPage-1)" :key="i" v-html="windowStart+n-1"></a>
            </li>
          </template>
        </template>
        <li :class="['btn-nav', $_css.linkClass, isOnLastPage ? $_css.disabledClass : '']">
          <a @click="loadPage('next')">
            <i v-if="$_css.icons.next != ''" :class="[$_css.icons.next]"></i>
            <span v-else>&rsaquo;&nbsp;</span>
          </a>
        </li>
        <li :class="['btn-nav', $_css.linkClass, isOnLastPage ? $_css.disabledClass : '']">
          <a @click="loadPage(lastPage)">
            <i v-if="$_css.icons.last != ''" :class="[$_css.icons.last]"></i>
            <span v-else>&raquo;</span>
          </a>
        </li>
      </ul>
    </div>

    <div class="form-group">
      <span class="info">{{total}} {{$t('table.item')}}, {{totalPage}} {{$t('table.page')}}</span>
    </div>

    <div class="form-group">
      <div class="input-group">
        <input type="number" class="form-control" min="1" :max="totalPage" v-model="current_page">
        <span class="input-group-btn">
          <button
            class="btn btn-primary"
            type="button"
            @click="loadPage(current_page)"
          >{{$t('table.go')}}</button>
        </span>
      </div>
    </div>
  </form>
</template>

<script>
import PaginationMixin from "./VuetablePaginationMixin.vue";

export default {
  mixins: [PaginationMixin]
};
</script>
<style>
.vuetable-pagination {
  background: #f9fafb !important;
}
</style>
