<template>
  <el-tree :data="PosPluList" :props="DCDLProps" @node-click="handleNodeClick"></el-tree>
</template>

<script>
export default {
  name: 'dishesGroup',
  data() {
    return {
      defaultProps: {
        children: 'plu_bList',
        label: 'name'
      },
      PosPluList: [
        { id: '01', name: '厨部', plu_bList: [{ code: '0100', name: '厨部' }] },
        { id: '02', name: '烧味', namee: 'namee', bgcolor: 'balck' },
        { id: '03', name: '点心', namee: 'namee', bgcolor: 'red' },
        { id: '04', name: '酸菜', namee: 'namee', bgcolor: 'balck' },
        { id: '05', name: '酒水', namee: 'namee', bgcolor: 'red' },
        { id: '06', name: '杂项', namee: 'namee', bgcolor: 'balck' },
        { id: '07', name: '酒席', namee: 'namee', bgcolor: 'red' }
      ]
    }
  }
}
</script>