<script>
export default {
  props: {
    css: {
      type: Object,
      default () {
        return {
          wrapperClass: 'pagination',
          activeClass: 'active',
          disabledClass: 'disabled',
          pageClass: '',
          linkClass: '',
          paginationClass: '',
          paginationInfoClass: '',
          dropdownClass: '',
          icons: {
            first: '',
            prev: '',
            next: '',
            last: '',
          }
        }
      }
    },
    onEachSide: {
      type: Number,
      default () {
        return 2
      }
    },
    tag: {
      type: String,
      default: 'main'
    }
  },
  data: function() {
    return {
      eventPrefix: 'vuetable-pagination:',
      tablePagination: null,
      current_page:''
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
        : this.tablePagination.last_page
    },
    isOnFirstPage () {
      return this.tablePagination === null
        ? false
        : this.tablePagination.current_page === 1
    },
    isOnLastPage () {
      return this.tablePagination === null
        ? false
        : this.tablePagination.current_page === this.tablePagination.last_page
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
        return this.totalPage - this.onEachSide*2
      }

      return this.tablePagination.current_page - this.onEachSide
    },
  },
  mounted(){
    //接收分页数据
    this.$parent.$on('vuetable:pagination-data-'+this.tag,tablePagination=>this.setPaginationData(tablePagination));
  },
  methods: {
    loadPage (page) {
      // 发送 切页事件
      this.$parent.$emit(this.eventPrefix+'change-page-'+this.tag, page)
    },
    isCurrentPage (page) {
      return page === this.tablePagination.current_page
    },
    setPaginationData (tablePagination) {
      this.current_page = ''
      this.tablePagination = tablePagination
    },
    resetData () {
      this.tablePagination = null
    }
  }
}
</script>
