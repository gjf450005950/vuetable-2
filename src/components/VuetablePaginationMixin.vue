<script>
import CssSemanticUI from './VuetableCssSemanticUI.js'

export default {
  props: {
    css: {
      type: Object,
      default () {
        return {}
      }
    },
    onEachSide: {
      type: Number,
      default () {
        return 2
      }
    },
    firstPage: {
      type: Number,
      default: 1
    },
    tag: {
      type: String,
      default: ''
    }
  },
  data: function() {
    return {
      eventPrefix: 'vuetable-pagination:',
      tablePagination: null,
      $_css: {},
      current_page:'' //新增 vic
    }
  },
  computed: {
     total () {
      return this.tablePagination === null
        ? 0
        : this.tablePagination.total
    },
    totalPage () {
      return this.tablePagination === null
        ? 0
        : this.tablePagination.last_page - this.firstPage + 1
    },
    lastPage () {
      return this.tablePagination === null
        ? 0
        : this.tablePagination.last_page
    },
    isOnFirstPage () {
      return this.tablePagination === null
        ? false
        : this.tablePagination.current_page === this.firstPage
    },
    isOnLastPage () {
      return this.tablePagination === null
        ? false
        : this.tablePagination.current_page === this.lastPage
    },
    notEnoughPages () {
      return this.totalPage < (this.onEachSide * 2) + 4
    },
    windowSize () {
      return this.onEachSide * 2 +1;
    },
    windowStart () {
      if (!this.tablePagination || this.tablePagination.current_page <= this.onEachSide) {
        return 1
      } else if (this.tablePagination.current_page >= (this.totalPage - this.onEachSide)) {
        return this.totalPage - this.onEachSide * 2
      }

      return this.tablePagination.current_page - this.onEachSide
    },
  },
  created () {
    this.mergeCss()
  },
  mounted(){
    //接收分页数据
    this.$parent.$on('vuetable:pagination-data:'+this.tag,tablePagination=>this.setPaginationData(tablePagination));
  },
  methods: {
    mergeCss () {
      this.$_css = {...CssSemanticUI.pagination, ...this.css}
    },
    loadPage (page) {
      //this.$emit(this.eventPrefix + 'change-page', page)
      // 发送 切页事件
      this.$parent.$emit('vuetable:change-page:'+this.tag, page)
    },
    isCurrentPage (page) {
      return page === this.tablePagination.current_page
    },
    setPaginationData (tablePagination) {
      this.current_page = ''//update vic
      this.tablePagination = tablePagination
    },
    resetData () {
      this.tablePagination = null
    }
  }
}
</script>
