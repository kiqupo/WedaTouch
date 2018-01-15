<template>
  <div>
  <el-table :data="DishesClassList" height="320" border style="width: 100%">
    <el-table-column prop="id" label="大类编码" sortable></el-table-column>
    <el-table-column prop="name" label="名称" sortable></el-table-column>
    <el-table-column prop="namee" label="英文名称"></el-table-column>
    <el-table-column label="背景颜色"><template slot-scope="scope"><svg-icon :style="{color: scope.row.bgcolor}" icon-class="square" /></template></el-table-column>
    <el-table-column label="操作">
      <template slot-scope="scope">
        <el-button type="primary" icon="el-icon-edit" size="mini" @click="EditDishesClass(scope.row)">编辑</el-button>
        <el-button icon="el-icon-delete" size="mini" type="danger" @click="SaveDishesClass(2, scope.row)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
  <div v-show="!SEdit" class="text-center A-margintop2p">
    <el-button @click="NewDishesClass()">新建大类</el-button>
    <el-button type="success">打印</el-button>
  </div>
  <div v-show="SEdit" class="text-center A-padding3p">
    <el-form ref="DCform" status-icon :rules="Rule_dclassForm" :model="DisDishesClass" style="margin-top:-2%;">
      <el-row :gutter="10">
        <el-col :span="12">
          <el-form-item prop="dcid" label="大类编码：">
            <el-input v-model="DisDishesClass.id" readonly></el-input>
          </el-form-item>
          <el-form-item prop="dcbgcolor" label="背景颜色：">
            <el-color-picker v-model="DisDishesClass.bgcolor"></el-color-picker>
          </el-form-item>
        </el-col>
        <el-col :span="12">
            <el-form-item prop="dcname" label="名称：">
              <el-input v-model="DisDishesClass.name"></el-input>
            </el-form-item>
            <el-form-item label="英文名称：">
              <el-input v-model="DisDishesClass.namee"></el-input>
            </el-form-item>
        </el-col>
      </el-row>
      <el-button @click="SaveDishesClass(0,DisDishesClass)" type="primary" :loading="Btnloading_savedc">保存</el-button>
      <el-button @click="CancelEdit()" type="info">取消</el-button>
    </el-form>
  </div>
  </div>
</template>

<script>
export default {
  name: 'dishesClass',
  data() {
    const vaDClassId = (rule, value, callback) => {
      console.log(value)
      if (value > 13) {
        callback(new Error('大类编号只能填01~13'))
      } else {
        callback()
      }
    }
    const vaDClassName = (rule, value, callback) => {
      console.log(value)
      if (!value) {
        return callback() // new Error('大类名称不能为空')
      } else {
        callback()
      }
    }
    const vaDClassBgcolor = (rule, value, callback) => {
      if (!value) {
        return callback() // new Error('背景颜色不能为空')
      } else {
        callback()
      }
    }
    return {
      DishesClassList: [
        { id: '01', name: '厨部', namee: 'namee', bgcolor: 'red' },
        { id: '02', name: '烧味', namee: 'namee', bgcolor: 'balck' },
        { id: '03', name: '点心', namee: 'namee', bgcolor: 'red' },
        { id: '04', name: '酸菜', namee: 'namee', bgcolor: 'balck' },
        { id: '05', name: '酒水', namee: 'namee', bgcolor: 'red' },
        { id: '06', name: '杂项', namee: 'namee', bgcolor: 'balck' },
        { id: '07', name: '酒席', namee: 'namee', bgcolor: 'red' }
      ],
      DisDishesClass: { id: '', name: '', namee: '', bgcolor: '' },
      SEdit: false, Btnloading_savedc: false,
      Rule_dclassForm: {
        dcid: [{ required: true, trigger: 'blur', validator: vaDClassId }],
        dcname: [{ trigger: 'blur', validator: vaDClassName }],
        dcbgcolor: [{ trigger: 'blur', validator: vaDClassBgcolor }]
      }
    }
  },
  methods: {
    EditDishesClass(dishclass) {
      this.DisDishesClass = JSON.parse(JSON.stringify(dishclass))
      this.SEdit = true
    },
    NewDishesClass() {
      /* eslint-disable no-new */
      var maxid = parseInt(this.DishesClassList.sort(this.DishesClassList.id)[this.DishesClassList.length - 1].id)
      if (maxid + 1 > 13) {
        this.$message({ message: '大类已满，不能再添加', type: 'warning' })
        return false
      }
      this.DisDishesClass = JSON.parse(JSON.stringify({ id: maxid + 1, name: '', namee: '', bgcolor: '' }))
      if (this.DisDishesClass.id < 10) {
        this.DisDishesClass.id = '0' + this.DisDishesClass.id
      } else {
        this.DisDishesClass.id += ''
      }
      this.SEdit = true
    },
    SaveDishesClass(way, disdclass) {
      if (way === 0) {
        console.log(this.DisDishesClass)
        this.$refs['DCform'].validate(valid => {
          if (valid) {
            this.DishesClassList.push(this.DisDishesClass)
            this.CancelEdit()
          } else {
            console.log('error submit!!')
            return false
          }
        })
      }
    },
    CancelEdit() {
      this.DisDishesClass = { id: '', name: '', namee: '', bgcolor: '' }
      this.$refs['DCform'].resetFields()
      this.SEdit = false
    }
  }
}
</script>