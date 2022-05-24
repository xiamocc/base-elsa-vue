<script>
import { toHyphenateEvent } from '../utils'
import { prefix } from '../config'
let windowHeight = parseInt(window.innerHeight)
export default {
  name: `${prefix}-table`,
  inheritAttrs: false,
  props: {
    columns: {
      type: Array,
      default() {
        return []
      },
    },
    dataSource: {
      type: Array,
      default() {
        return []
      },
    },
    // 分页配置
    pagination: {
      type: Object,
      default() {
        return {}
      },
    },
    config: {
      type: Object,
      default() {
        return {
          diffHeight: 440,
        }
      },
    },
  },
  data() {
    return {
      windowHeight: windowHeight,
      autoHeight: {
        height: '',
      },
    }
  },
  created() {
    window.addEventListener('resize', this.getHeight)
    this.getHeight()
  },
  destroyed() {
    window.removeEventListener('resize', this.getHeight)
  },
  methods: {
    getHeight() {
      this.autoHeight.height = windowHeight - this.config.diffHeight + 'px'
    },
    buildElColumnRender(column) {
      const { customRender, ...rest } = column
      if (customRender) {
        // 获取 <easy-table>...</easy-table> 内声明的非匿名插槽
        const customRenderFn = this.$scopedSlots[customRender]
        if (customRenderFn) {
          return this.$createElement('el-table-column', {
            props: rest,
            scopedSlots: {
              default: ({ row, column, $index }) => {
                return customRenderFn({ row, column, $index })
              },
            },
          })
        } else {
          console.warn(`未正确配置自定义 customRender 模板: ${customRender}`)
        }
      }
      return this.$createElement('el-table-column', { props: rest })
    },
    renderTable() {
      const { dataSource, columns } = this.$props
      // https://element.eleme.io/#/zh-CN/component/table#table-attributes
      const elProps = this.$attrs
      elProps.data = dataSource
      elProps.height = this.autoHeight.height
      const tableRows = columns.reduce((acc, column) => {
        acc.push(this.buildElColumnRender(column))
        return acc
      }, [])
      return this.$createElement(
        'el-table',
        {
          props: elProps,
          on: toHyphenateEvent(this.$listeners),
        },
        tableRows
      )
    },
    renderPagination() {
      if (this.pagination) {
        return this.$createElement(`${prefix}-pagination`, {
          props: this.pagination,
        })
      }
      return ''
    },
  },
  render: function (h) {
    return h(
      'div',
      {
        class: `${prefix}-table`,
        style: {
          width: '100%',
          height: '100%',
        },
      },
      [this.renderTable(), this.renderPagination()]
    )
  },
}
</script>
