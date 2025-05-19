<template>
  <div class="common-layout">
    <el-container>
      <el-header class="header">
        <div class="header-left">
          <div class="title-container">
            <h2 class="title">生态聚合服务平台</h2>
            <h3 class="subtitle">ECOLOGICAL AGGREGATION SERVICE PLATFORM</h3>
          </div>
        </div>
      </el-header>
      <el-container class="content-container">
        <el-aside class="left-sidebar" width="220px">
          <el-menu default-active="需求广场" :collapse-transition="false">
          <!-- 首页 -->
          <el-menu-item index="首页">
            <el-icon><HomeFilled /></el-icon>
            <span>首页</span>
          </el-menu-item>
          
          <!-- 待办事项 -->
          <el-menu-item index="待办事项">
            <el-icon><Check /></el-icon>
            <span>待办事项</span>
          </el-menu-item>
          
          <!-- 供应商管理 -->
          <el-sub-menu index="供应商管理">
            <template #title>
              <el-icon><User /></el-icon>
              <span>供应商管理</span>
            </template>
          </el-sub-menu>
          
          <!-- 核心管理 -->
          <el-sub-menu index="核心管理">
            <template #title>
              <el-icon><Folder /></el-icon>
              <span>核心管理</span>
            </template>
            <el-menu-item index="供应商填报与修改">供应商填报与修改</el-menu-item>
            <el-menu-item index="需求反馈创建">需求反馈创建</el-menu-item>
            <el-menu-item index="需求广场">需求广场</el-menu-item>
          </el-sub-menu>
          
          <!-- 系统管理 -->
          <el-sub-menu index="系统管理">
            <template #title>
              <el-icon><Setting /></el-icon>
              <span>系统管理</span>
            </template>
          </el-sub-menu>
          
          <!-- 工作流程 -->
          <el-sub-menu index="工作流程">
            <template #title>
              <el-icon><Calendar /></el-icon>
              <span>工作流程</span>
            </template>
          </el-sub-menu>
          
          <!-- 报表管理 -->
          <el-sub-menu index="报表管理">
            <template #title>
              <el-icon><Document /></el-icon>
              <span>报表管理</span>
            </template>
          </el-sub-menu>
          
          <!-- 基础设施 -->
          <el-sub-menu index="基础设施">
            <template #title>
              <el-icon><Tools /></el-icon>
              <span>基础设施</span>
            </template>
          </el-sub-menu>
          </el-menu>
        </el-aside>
        <el-main class="scrollable-content">
        <div class="breadcrumb">
          <el-breadcrumb separator="/">
            <el-breadcrumb-item>首页</el-breadcrumb-item>
            <el-breadcrumb-item>需求广场</el-breadcrumb-item>
          </el-breadcrumb>
        </div>

        <div class="search-area">
          <div class="search-container">
            <el-row :gutter="20">
              <el-col :span="6">
                <el-input
                  placeholder="请输入需求名称"
                  v-model="searchForm.name"
                  clearable>
                  <template #prepend>需求名称</template>
                </el-input>
              </el-col>
              <el-col :span="6">
                <el-input
                  placeholder="请输入需求人姓名"
                  v-model="searchForm.personName"
                  clearable>
                  <template #prepend>需求人姓名</template>
                </el-input>
              </el-col>
              <el-col :span="6">
                <el-date-picker
                  v-model="searchForm.dateRange"
                  type="daterange"
                  range-separator="至"
                  start-placeholder="开始日期"
                  end-placeholder="结束日期">
                </el-date-picker>
              </el-col>
            </el-row>
          </div>
          <div class="search-buttons">
            <el-row :gutter="20">
              <el-col :span="6">
                <div style="display: flex;">
                  <el-button type="primary" @click="handleSearch" style="margin-right: 10px;">搜索</el-button>
                  <el-button @click="handleReset">重置</el-button>
                </div>
              </el-col>
            </el-row>
          </div>
        </div>

        <div class="table-area">
          <el-table
            :data="tableData"
            style="width: 100%"
            border
            @row-click="handleRowClick"
            :row-class-name="getRowClass">
            <el-table-column
              prop="name"
              label="需求名称"
              width="180">
            </el-table-column>
            <el-table-column
              prop="personName"
              label="需求人姓名"
              width="120">
            </el-table-column>
            <el-table-column
              prop="industry"
              label="行业"
              width="120">
            </el-table-column>
            <el-table-column
              prop="region"
              label="区域"
              width="120">
            </el-table-column>
            <el-table-column
              prop="contact"
              label="需求人联系方式"
              width="150">
            </el-table-column>
            <el-table-column
              prop="amount"
              label="预期金额（万元）"
              width="120">
            </el-table-column>
            <el-table-column
              prop="expectedTime"
              label="期望完成时间"
              width="180">
            </el-table-column>
            <el-table-column
              prop="completionStatus"
              label="完成状态"
              width="100">
              <template #default="scope">
                <el-tag :type="scope.row.completionStatus === '已完成' ? 'success' : 'info'">
                  {{ scope.row.completionStatus }}
                </el-tag>
              </template>
            </el-table-column>
            <el-table-column
              prop="publishStatus"
              label="发布状态"
              width="100">
              <template #default="scope">
                <el-tag :type="scope.row.publishStatus === '已发布' ? 'success' : 'info'">
                  {{ scope.row.publishStatus }}
                </el-tag>
              </template>
            </el-table-column>
            <el-table-column
              prop="publishTime"
              label="发布时间"
              width="120">
            </el-table-column>
            <el-table-column
              label="操作"
              width="180"
              class-name="fixed-column">
              <template #default="scope">
                <el-button type="text" @click="viewDetails(scope.row)">详情</el-button>
                <el-button type="text" @click="handleEdit(scope.row)">编辑</el-button>
                <el-button type="text" @click="handleDelete(scope.row)">删除</el-button>
              </template>
            </el-table-column>
          </el-table>
          <div class="pagination">
            <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="pagination.currentPage"
              :page-sizes="[10, 20, 50, 100]"
              :page-size="pagination.pageSize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="pagination.total">
            </el-pagination>
          </div>
        </div>
      </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  name: 'Demand',
  data() {
    return {
      // 搜索框功能相关数据
      searchForm: {
        name: '',
        personName: '',
        dateRange: null
      },
      // 表格数据
      tableData: [
        {
          name: '搜索功能增强',
          personName: '叶清欢',
          industry: '数字教育/云平台',
          region: '河北省 / 石家庄',
          contact: '13700137000',
          amount: 6,
          expectedTime: '2025-05-13 00:00:00',
          completionStatus: '未完成',
          publishStatus: '已发布',
          publishTime: '2025-05-15'
        },
        {
          name: '系统优化',
          personName: '张三',
          industry: '信息技术',
          region: '北京市 / 朝阳区',
          contact: '13800138000',
          amount: 10,
          expectedTime: '2025-06-01 00:00:00',
          completionStatus: '未完成',
          publishStatus: '已发布',
          publishTime: '2025-05-10'
        },
        {
          name: '功能扩展',
          personName: '李四',
          industry: '金融服务',
          region: '上海市 / 浦东新区',
          contact: '13900139000',
          amount: 8,
          expectedTime: '2025-07-15 00:00:00',
          completionStatus: '未完成',
          publishStatus: '已发布',
          publishTime: '2025-05-05'
        }
      ],
      // 分页数据
      pagination: {
        currentPage: 1,
        pageSize: 10,
        total: 3
      },
      // 选中的行
      selectedRow: null,
      // 对话框相关数据
      dialogVisible: false,
      detailForm: {},
      isEditing: false,
      originalData: null
    }
  },
  methods: {
    // 搜索表单功能相关方法
    handleSearch() {
      console.log('搜索功能:', this.searchForm)
      const filteredData = this.tableData.filter(item => {
        return (this.searchForm.name ? item.name.includes(this.searchForm.name) : true) &&
               (this.searchForm.personName ? item.personName.includes(this.searchForm.personName) : true) &&
               (!this.searchForm.dateRange || 
                 (new Date(item.expectedTime) >= new Date(this.searchForm.dateRange[0]) && 
                  new Date(item.expectedTime) <= new Date(this.searchForm.dateRange[1])) )
      })
      this.pagination.total = filteredData.length
      this.pagination.currentPage = 1
      console.log('搜索结果:', filteredData)
    },
    // 重置按钮的点击事件处理函数
    handleReset() {
      this.searchForm = {
        name: '',
        personName: '',
        dateRange: null
      }
    },
    // 查看需求详情的处理函数
    viewDetails(row) {
      this.detailForm = {...row}
      this.originalData = {...row}
      this.isEditing = false
      this.dialogVisible = true
    },
    // 编辑需求的处理函数
    handleEdit(row) {
      this.detailForm = {...row}
      this.originalData = {...row}
      this.isEditing = true
      this.dialogVisible = true
    },
    // 删除需求的处理函数
    handleDelete(row) {
      this.$confirm('此操作将永久删除该需求, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        const index = this.tableData.findIndex(item => item.name === row.name)
        if (index !== -1) {
          this.tableData.splice(index, 1)
          this.pagination.total -= 1
          this.$message({
            type: 'success',
            message: '删除成功!'
          })
        }
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    },
    // 保存修改的处理函数
    saveChanges() {
      if (this.detailForm.name === this.originalData.name && 
          JSON.stringify(this.detailForm) !== JSON.stringify(this.originalData)) {
        const index = this.tableData.findIndex(item => item.name === this.detailForm.name)
        if (index !== -1) {
          this.tableData[index] = {...this.detailForm}
          this.$message({
            type: 'success',
            message: '修改成功!'
          })
        }
      }
      this.dialogVisible = false
    },
    // 分页相关方法
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`)
      this.pagination.pageSize = val
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`)
      this.pagination.currentPage = val
    },
    // 点击表格行的处理函数
    handleRowClick(row) {
      this.selectedRow = row
    },
    // 为表格行添加样式的函数
    getRowClass({ row, rowIndex }) {
      return row === this.selectedRow ? 'selected-row' : ''
    }
  }
}
</script>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 15px;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  height: 60px !important;
}

.header-left {
  display: flex;
  align-items: center;
}

.title-container {
  display: flex;
  flex-direction: column;
}

.title {
  color: rgb(63, 62, 62);
  font-family: 黑体;
  font-size: 22px;
  font-weight: bold;
  margin: 0;
  margin-right: 5px;
}

.subtitle {
  color: #999;
  font-size: 8px;
  margin: 0;
}

.content-container {
  display: flex;
  height: calc(100vh - 60px);
}

.left-sidebar {
  width: 220px;
  background-color: #f5f7fa;
  position: fixed;
  height: calc(100vh - 60px);
  overflow-x:hidden;
  overflow-y: auto;
}

.scrollable-content {
  flex: 1;
  padding: 20px;
  margin-left: 220px;
  overflow-y: auto;
  background-color: #f5f7fa;
}

.breadcrumb {
  margin-bottom: 15px;
}

.search-area {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
  margin-bottom: 20px;
}

.search-container {
  justify-content: center;
  align-items: center;
  margin-bottom: 10px;
}

.search-buttons {
  margin-left: 0px;
}

.search-buttons .el-button {
  margin-left: 0px;
}

.table-area {
  background-color: #fff;
  padding: 15px;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
}

.pagination {
  justify-content: flex-end;
  margin-top: 15px;
  text-align: right;
}

/* 选中行样式 */
.selected-row {
  background-color: #f0f7ff !important;
}

/* 面包屑样式 */
.el-breadcrumb-item.is-active {
  color: #409eff;
  font-weight: bold;
}

/* 对话框样式 */
.el-dialog {
  border-radius: 8px;
  overflow: hidden;
}

.el-dialog__header {
  background-color: #409eff;
  color: white;
  padding: 15px 20px;
}

/* 固定操作列 */
::v-deep .fixed-column {
  position: sticky;
  right: 0;
  z-index: 10;
  background-color: #fff;
}

/* 分页样式 */
::v-deep .el-pagination {
  display: flex;
  justify-content: flex-end;
}

::v-deep .el-pagination__total,
::v-deep .el-pagination__sizes,
::v-deep .el-pagination__jumper {
  display: flex;
  align-items: center;
}
</style>
