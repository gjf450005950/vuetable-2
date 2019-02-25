<template>
  <form class="form-inline text-right">
    <div class="form-group">
      <ul v-show="tablePagination && tablePagination.last_page > 1" :class="css.wrapperClass">
        <li :class="['btn-nav', css.linkClass, isOnFirstPage ? css.disabledClass : '']">
          <a @click="loadPage(1)">
            <i v-if="css.icons.first != ''" :class="[css.icons.first]"></i>
            <span v-else>&laquo;</span>
          </a>
        </li>
        <li :class="['btn-nav', css.linkClass, isOnFirstPage ? css.disabledClass : '']">
          <a @click="loadPage('prev')">
            <i v-if="css.icons.next != ''" :class="[css.icons.prev]"></i>
            <span v-else>&nbsp;&lsaquo;</span>
          </a>
        </li>
        <template v-if="notEnoughPages">
          <template v-for="n in totalPage">
            <li :class="[css.pageClass, isCurrentPage(n) ? css.activeClass : '']">
              <a @click="loadPage(n)"
                 v-html="n">
              </a>
            </li>
          </template>
        </template>
        <template v-else>
          <template v-for="n in windowSize">
            <li :class="[css.pageClass, isCurrentPage(windowStart+n-1) ? css.activeClass : '']">
              <a @click="loadPage(windowStart+n-1)"
                 v-html="windowStart+n-1">
              </a>
            </li>
          </template>
        </template>
        <li :class="['btn-nav', css.linkClass, isOnLastPage ? css.disabledClass : '']">
          <a @click="loadPage('next')">
            <i v-if="css.icons.next != ''" :class="[css.icons.next]"></i>
            <span v-else>&rsaquo;&nbsp;</span>
          </a>
        </li>
        <li :class="['btn-nav', css.linkClass, isOnLastPage ? css.disabledClass : '']">
          <a @click="loadPage(totalPage)">
            <i v-if="css.icons.last != ''" :class="[css.icons.last]"></i>
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
          <button class="btn btn-default" type="button" @click="loadPage(current_page)">{{$t('table.go')}}</button>
        </span>
      </div>
    </div>
  </form>
</template>

<script>
  import PaginationMixin from './VuetablePaginationMixin.vue'

  export default {
    mixins: [PaginationMixin],
  }
</script>
