<template>
  <div>
    <el-alert type="warning" title="virtual-column组件设type属性为 expand" show-icon></el-alert>
    <virtual-scroll
      ref="virtualScroll"
      :data="list"
      :item-size="62"
      key-prop="id"
      @change="onVirtualChange">
      <el-table
        ref="table"
        v-loading="loading"
        :data="tableData"
        height="600"
        row-key="id"
        style="width: 100%"
        :default-expand-all="false"
        :expand-row-keys="expandRowKeys"
        @expand-change="handleExpandChange">
        <virtual-column type="expand">
          <template slot-scope="props">
            <el-form label-position="left" class="demo-table-expand">
              <el-form-item label="商品名称">
                <span>{{ props.row.name }}</span>
              </el-form-item>
              <el-form-item label="所属店铺">
                <span>{{ props.row.shop }}</span>
              </el-form-item>
              <el-form-item label="商品 ID">
                <span>{{ props.row.id }}</span>
              </el-form-item>
              <el-form-item label="店铺 ID">
                <span>{{ props.row.shopId }}</span>
              </el-form-item>
              <el-form-item label="商品分类">
                <span>{{ props.row.category }}</span>
              </el-form-item>
              <el-form-item label="店铺地址">
                <span>{{ props.row.address }}</span>
              </el-form-item>
              <el-form-item label="商品描述">
                <span>{{ props.row.desc }}</span>
              </el-form-item>
              <el-form-item label="商品描述">
                <span>{{ props.row.desc }}</span>
              </el-form-item>
              <el-form-item label="商品描述">
                <span>{{ props.row.desc }}</span>
              </el-form-item>
              <el-form-item label="商品描述">
                <span>{{ props.row.desc }}</span>
              </el-form-item>
            </el-form>
          </template>
        </virtual-column>
        <el-table-column
          label="商品 ID"
          prop="id">
        </el-table-column>
        <el-table-column
          label="商品名称"
          prop="text">
        </el-table-column>
        <el-table-column
          label="描述"
          prop="text2">
        </el-table-column>
      </el-table>
    </virtual-scroll>
    <div style="margin-top: 20px">
      <el-button @click="toggleExpand([list[1], list[2]])">切换第二、第三行的展开收起状态</el-button>
      <el-button @click="toggleExpand(list, false)">全部收起</el-button>
      <el-button @click="setExpandRowKeys()">setExpandRowKeys[4, 5, 6, 7]</el-button>
      <el-button @click="insert()">insert</el-button>
    </div>
  </div>
</template>

<script>
import VirtualScroll, { VirtualColumn } from 'el-table-virtual-scroll'

export default {
  components: {
    VirtualScroll,
    VirtualColumn
  },
  data () {
    return {
      list: [],
      tableData: [],
      loading: false,
      expandRowKeys: [1, 2]
    }
  },
  methods: {
    fetchData () {
      this.loading = true
      this.list = []
      setTimeout(() => {
        this.list = []
        for (let i = 1; i < 2000; i++) {
          const text = this.getRandomContent()
          const text2 = this.getRandomContent()
          this.list.push({
            id: i,
            show: false,
            text,
            text2
          })
        }
        this.loading = false
      }, 1000)
    },
    getRandomContent () {
      const content = [
        '这是一条测试数据',
        '君不见黄河之水天上来，奔流到海不复回。',
        '十年生死两茫茫',
        '寻寻觅觅，冷冷清清，凄凄惨惨戚戚。',
        '桃花坞里桃花庵，桃花庵里桃花仙；桃花仙人种桃树，又摘桃花卖酒钱。',
        '明月几时有，把酒问青天。',
        '槛菊愁烟兰泣露，罗幕轻寒，',
        '寒蝉凄切，对长亭晚，骤雨初歇。都门帐饮无绪，留恋处，兰舟催发。执手相看泪眼，竟无语凝噎。念去去，千里烟波，暮霭沉沉楚天阔。多情自古伤离别，更那堪冷落清秋节！今宵酒醒何处？杨柳岸，晓风残月。此去经年，应是良辰好景虚设。便纵有千种风情，更与何人说？',
        '红豆生南国，春来发几枝。',
        '黄鹂'
      ]
      const i = Math.floor(Math.random() * 10)
      return content[i]
    },
    async onVirtualChange (virtualList) {
      this.tableData = virtualList
    },
    toggleExpand (rows, expanded) {
      rows.forEach(row => {
        this.$refs.virtualScroll.toggleRowExpansion(row, expanded)
      })
    },
    handleExpandChange (row, expandedRows) {
      console.log('handleExpandChange', row, expandedRows.map(i => i.id))
    },
    setExpandRowKeys () {
      this.expandRowKeys = [4, 5, 6, 7]
    },
    insert () {
      const last = this.list.at(-1)
      const text = this.getRandomContent()
      const text2 = this.getRandomContent()
      this.list.push({
        id: last.id + 1,
        show: false,
        text,
        text2
      })
    }
  },
  created () {
    this.fetchData()
  }
}
</script>

<style lang='less' scoped>
.demo-table-expand {
  font-size: 0;
}
.demo-table-expand label {
  width: 90px;
  color: #99a9bf;
}
.demo-table-expand .el-form-item {
  margin-right: 0;
  margin-bottom: 0;
  width: 50%;
}
</style>
