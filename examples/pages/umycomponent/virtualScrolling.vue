<template>
  <div>
    <h3>虚拟滚动(基础虚拟，不解决列多情况)</h3>
    <p>建议开启使用：show-body-overflow 和 show-header-overflow 。</p>
    <p>（注意：不给height或者不给maxheight，又或者给的是0，use-virtual直接会关闭）。 如果你数据多而且高度为0或者为空，那么就会卡死） 。</p>
    <p>(如果你需要数据多少表格就多高，自适应，那么就不要给高度，且虚拟滚动会关闭。这个不支持数据量太大的情况哦)</p>
    <p>（注：启用纵向虚拟后不支持动态行高，如果要支持动态行高，将虚拟滚动关闭即可）。</p>
    <p>（如果想滚动表格到某个位置你可以调用pagingScrollTopLeft方法。参数可以看文档api哦）。</p>
    <p>当`u-table`元素中注入`data`对象数组后，添加use-virtual属性开启虚拟，同时设置row-height行高(它有默认值，请看文档)，同时必须给定height，或者max-height。</p>
    <p>注意：row-height不是去为了设置表格行高，它只是为了表格里面计算某些东西。如果你需要改变你的行高，你需要去写css改变td的高度即可！即可参考row-height的api描述</p>
    <div>
      <p style="margin: 20px 0;">
        <el-button @click="setHei(400)">设置高度为400</el-button>
        <el-button @click="setHei(600)">设置高度为600</el-button>
        <el-button @click="setHei(800)">设置高度为800</el-button>
        <el-button @click="pagingScrollTopLeft(2000)">滚动到2千位置</el-button>
        <el-button @click="pagingScrollTopLeft(5000)">滚动到5千位置</el-button>
        <el-button @click="pagingScrollTopLeft(0)">滚动到顶部</el-button>
        <el-button @click="scrollBottom">滚动到底部位置</el-button>
      </p>
      <u-table
        ref="plTable"
        :data="tableData"
        :height="height"
        use-virtual
        showBodyOverflow="title"
        showHeaderOverflow="title"
        :row-height="rowHeight"
        border>
        <u-table-column type="index" width="100" fixed/>
        <u-table-column
          v-for="item in columns"
          :key="item.id"
          :resizable="item.resizable"
          :show-overflow-tooltip="item.showOverflow"
          :prop="item.prop"
          :label="item.label"
          :fixed="item.fixed"
          :width="item.width"/>
        <u-table-column
          fixed="right"
          label="操作"
          width="150">
          <template slot-scope="scope">
            <el-button type="text" size="small">查看</el-button>
            <el-button type="text" size="small">编辑</el-button>
          </template>
        </u-table-column>
      </u-table>
    </div>
    <div v-html="md" style="margin-top: 30px" class="width100"></div>
  </div>
</template>

<script>
  import md2 from '../../components/u-table-component/virtualTableUsage/virtualScrolling.md'
  export default {
    computed: {
      md () {
        return md2
      }
    },
    data() {
      return {
        height: 0,
        rowHeight: 55, // 如果你这里给行高为50，那么你表格行会出现错乱，不要问为啥，因为你可以看看控制台看节点的高是多少，是55，而你这里给50就有问题！
        columns: Array.from({ length: 8 }, (_, idx) => ({
          prop: 'address', id: idx, label: '地址地址地址地址地址地址地址地址地址地址地址' + idx, width: 200
        })),
        tableData: Array.from({ length: 500 }, (_, idx) => ({
          id: idx + 1,
          date: '2016-05-03',
          name: 1,
          ab: '欢迎使用u-table',
          address: '北京市天安门天安门天安门北京市天安门天安门天安门北京市天安门天安门天安门'
        }))
      }
    },
    mounted () {
      this.height = 500 // 动态设置高度
    },
    methods: {
      setHei (val) {
        this.height = val
      },
      scrollBottom () {
        this.$refs.plTable.scrollBottom()
      },
      pagingScrollTopLeft (val) {
        this.$refs.plTable.pagingScrollTopLeft(val, 0)
      },
    }
  }
</script>
