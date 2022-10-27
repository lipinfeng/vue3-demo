<template>
  <div class="table-box">
    <!--标题-->
    <div class="title">
      <h2>CURD DEMO</h2>
    </div>
    <!--query-->
    <div class="query-box">
      <el-input class="query-input" v-model="queryInput" placeholder="请输入姓名搜索🔍" @input="handleQueryName"></el-input>
     <div class="btn-list">
       <el-button type="danger" @click="handleDelList" v-if="multipleSelection.length > 0">删除多选</el-button>
       <el-button type="primary" @click="handleAdd">增加</el-button>
     </div>
    </div>
    <!--table-->
    <el-table
        ref="multipleTableRef"
        :data="tableData"
        style="width: 100%"
        @selection-change="handleSelectionChange" border>
      <el-table-column type="selection" width="55"/>
      <el-table-column prop="name" label="姓名" width="80"/>
      <el-table-column prop="email" label="邮箱" width="195"/>
      <el-table-column prop="phone" label="电话" width="160"/>
      <el-table-column prop="state" label="状态" width="80"/>
      <el-table-column prop="address" label="地址" width="280"/>
      <el-table-column fixed="right" label="操作" width="150">
        <template #default="scope">
          <el-button type="danger" size="small" @click="handleRowDel(scope.row)">删除</el-button>
          <el-button type="primary" size="small" @click="handleEdit(scope.row)">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!--dialog-->
    <el-dialog v-model="dialogFormVisible" :title="dialogType === 'add'? '新增' : '编辑'" width="30%">
      <el-form :model="tableForm">
        <el-form-item label="姓名：" :label-width="80">
          <el-input v-model="tableForm.name" autocomplete="off"/>
        </el-form-item>
        <el-form-item label="邮箱：" :label-width="80">
          <el-input v-model="tableForm.email" autocomplete="off"/>
        </el-form-item>
        <el-form-item label="电话：" :label-width="80">
          <el-input v-model="tableForm.phone" autocomplete="off"/>
        </el-form-item>
        <el-form-item label="状态：" :label-width="80">
          <el-input v-model="tableForm.state" autocomplete="off"/>
        </el-form-item>
        <el-form-item label="地址：" :label-width="80">
          <el-input v-model="tableForm.address" autocomplete="off"/>
        </el-form-item>
      </el-form>
      <template #footer>
      <span class="dialog-footer">
        <el-button type="primary" @click="dialogConfirm">
          确认
        </el-button>
      </span>
      </template>
    </el-dialog>
  </div>
</template>

<script setup>
import {reactive, ref} from "vue";

//数据
let queryInput = $ref("")
let tableData = $ref([
  {
    id: '1',
    name: 'Tom1',
    email: '123456789@qq.com',
    phone: '987654321',
    state: '在职',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id: '2',
    name: 'Tom2',
    email: '123456789@qq.com',
    phone: '987654321',
    state: '在职',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id: '3',
    name: 'Tom3',
    email: '123456789@qq.com',
    phone: '987654321',
    state: '在职',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id: '4',
    name: 'Tom4',
    email: '123456789@qq.com',
    phone: '987654321',
    state: '在职',
    address: 'No. 189, Grove St, Los Angeles',
  },
  {
    id: '5',
    name: 'Tom5',
    email: '123456789@qq.com',
    phone: '987654321',
    state: '在职',
    address: 'No. 189, Grove St, Los Angeles',
  },
])
let multipleSelection = $ref([])
let dialogFormVisible = $ref(false)
let tableForm = $ref({
  name: '',
  email: '',
  phone: '',
  state: '',
  address: ''
})
let dialogType = $ref('add')
let tableDataCopy = Object(tableData)

//方法
const handleRowDel = ({id}) => {
  // console.log(id)
  //通过 id 找到相应的数据
  let index = tableData.findIndex(item => item.id === id)
  //删除
  tableData.splice(index, 1)
}
const handleDelList = () => {
  multipleSelection.forEach(id => {
    handleRowDel({id})
  })
  multipleSelection = []
}
//选择框
const handleSelectionChange = (val) => {
  // multipleSelection.value = val
  multipleSelection = []
  val.forEach(item => {
    multipleSelection.push(item.id)
  })
}
//添加数据的模态框
const handleAdd = () => {
  dialogFormVisible = true
  tableForm = {}
  dialogType = 'add'
}
//编辑
const handleEdit = (row) => {
  dialogFormVisible = true
  dialogType = 'edit'
  tableForm = {...row}
}
//保存并添加模态框的数据
const dialogConfirm = () => {
  dialogFormVisible = false
  //拿到数据
  //添加到 table
  tableData.push({
    id: (tableData.length + 1).toString(),
    ...tableForm
  })
}
//搜索
const handleQueryName = (val) =>{
  if (val.length > 0) {
    tableData = tableData.filter(item => (item.name).toLowerCase().match(val.toLowerCase()))
  }else {
    tableData = tableDataCopy
  }
}

</script>

<style scoped>

.table-box {
  width: 1000px;
  margin: 150px auto;
}

.title {
  text-align: center;
}

.query-box {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.query-input {
  width: 200px;
}
</style>
