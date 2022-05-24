<script>
import { toHyphenateEvent } from '../utils'
import { prefix } from '../config'

export default {
  name: `${prefix}-pagination`,
  props: {
    total: {
      type: Number,
      default: 0,
    },
    currentPage: {
      type: Number,
      default: 1,
    },
    pageSize: {
      type: Number,
      default: 10,
    },
    pageSizes: {
      type: Array,
      default() {
        return [10, 20, 30, 50]
      },
    },
    layout: {
      type: String,
      default: 'total, sizes, prev, pager, next, jumper',
    },
    background: {
      type: Boolean,
      default: true,
    },
    autoScroll: {
      type: Boolean,
      default: true,
    },
    hidden: {
      type: Boolean,
      default: false,
    },
    currentChange: {
      type: Function,
      default() {
        return () => {}
      },
    },
    prevClick: {
      type: Function,
      default() {
        return () => {}
      },
    },
    nextClick: {
      type: Function,
      default() {
        return () => {}
      },
    },
    sizeChange: {
      type: Function,
      default() {
        return () => {}
      },
    },
  },
  render: function (h) {
    const { currentChange, prevClick, nextClick, sizeChange, ...elProps } = this.$props
    if (this.$props.total) {
      return h('el-pagination', {
        style: {
          margin: '16px 12px',
          'text-align': 'right',
        },
        props: elProps,
        on: toHyphenateEvent({ currentChange, prevClick, nextClick, sizeChange }),
      })
    }
    return null
  },
}
</script>
