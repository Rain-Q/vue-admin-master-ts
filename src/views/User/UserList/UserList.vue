<template lang="pug">
  div#table-page
    hl-table(
      :tableList="tableList"
    )
</template>
<script lang="ts">
  import { Vue, Component } from 'vue-property-decorator'
  import hlTable from '@/components/hl-table/hl-table.vue'
  import { getPage } from '../../../api/user/index'
  @Component({
    name: 'UserList',
    components: {
      hlTable
    }
  })
  export default class UserList extends Vue {
    private tableList: {
      expandList: Array<any>,
      dataList: Array<any>,
      total: Number,
      form: {
        pageNo: Number,
        pageSize: Number,
        query: {
          role: Number
        }
      }
    } = {
      // 扩展列表
      expandList: [{
        prop: 'id',
        label: 'ID',
        render: false,
      }, {
        prop: 'avatarUrl',
        label: '用户头像',
        render: (h: any, row: any) => {
          return h('img', {
            attrs: {
              'src': row.avatarUrl,
              'style': 'width: 64px;height: 64px;'
            }
          })
        }
      }, {
        prop: 'openid',
        label: '微信openid',
        width: 240,
        render: false
      }, {
        prop: 'nickName',
        label: '用户名称',
        render: false
      }, {
        prop: 'city',
        label: '城市',
        width: 'auto',
        render: false
      }, {
        prop: 'update_time',
        label: '更新时间',
        width: 'auto',
        render: false
      }],
      // 列表数据
      dataList: [],
      total: 1,
      form: {
        pageNo: 1,
        pageSize: 20,
        query: {
          role: 1
        }
      }
    }
    created () {
      this.getPage()
    }
    async getPage () {
      let res = await getPage(this.tableList.form)
      this.$set(this.tableList, 'dataList', res.data)
      this.$set(this.tableList, 'total', res.total)
    }
  }
</script>