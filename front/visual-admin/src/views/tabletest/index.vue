<template>
  <div class="app-container">
    <pre>{{ listJson.total }}</pre>
    <table>

      <tr>
        <th v-for="col in columns" :key="col">
          {{ col | capitalize }}
        </th>
      </tr>

      <tr v-for="entry in list" :key="entry.id">
        <td>{{ entry.id }}</td>
        <td>{{ entry.title }}</td>

        <td>{{ entry.author }}</td>
        <td>{{ entry.pageviews }}</td>
        <td>{{ entry.status | statusmap }}</td>
        <!-- <td>{{ entry.display_time }}</td> -->
        <td><img src='https://img2.baidu.com/it/u=1390346698,3472037906&fm=253&fmt=auto&app=138&f=JPEG?w=290&h=290' width="50" height="60" />
        https://img2.baidu.com/it/u=1390346698,3472037906&fm=253&fmt=auto&app=138&f=JPEG?w=290&h=290</td>
      </tr>

    </table>
  </div>
</template>

<script>
import { getList } from '@/api/table';

export default {
  filters: {
    statusmap(status){
      if(status=='publish') return 'success';
      else if(status == 'draft') return 'gray';
      else return 'danger1';
    },
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'gray',
        deleted: 'danger',
      };
      return statusMap[status];
    },
  },
  data() {
    return {
      list: null,
      listLoading: true,
      listJson: null,

      columns:['ID', 'Title', 'Author', 'Pageviews', 'Status','Display_time'],
    };
  },
  created() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      this.listLoading = true;
      getList().then((response) => {
        this.list = response.data.items;
        this.listLoading = false;
        this.listJson = response.data;
      });
    },
  },
};
</script>
