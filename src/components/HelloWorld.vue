<template>
  <div>
		<div style="margin-left: 15px;">
			<h3>表格</h3>
		</div>
		<vxe-toolbar style="margin-left: 10px;">
			<template v-slot:buttons>
				<vxe-button @click="insertEvent()">新增</vxe-button>
				<vxe-button @click="removeEvent()">删除选中</vxe-button>
				<vxe-button status="primary" @click="selectWj()">添加附件</vxe-button>
			</template>
		</vxe-toolbar>
		<div style="width: 1320px;">
			<vxe-table
				border
				show-overflow
				resizable
				size="medium"
				ref="xTable"
				class="my_table_insert"
				highlight-hover-row
				highlight-current-row
				max-height="400"
				:data="tableData.slice((currentPage- 1)*pageSize,currentPage*pageSize)"
				:edit-config="{trigger: 'click', mode: 'cell', icon: 'fa fa-pencil'}"
				>
				<vxe-table-column type="checkbox" width="60" fixed="left"></vxe-table-column>
				<vxe-table-column type="seq" width="60" fixed="left"></vxe-table-column>
				<vxe-table-column field="name" title="Name" width="200" fixed="left" sortable :edit-render="{name: 'input', defaultValue: '默认的名字'}"></vxe-table-column>
				<vxe-table-column field="typ" title="Type" width="200"></vxe-table-column>
				<vxe-table-column field="size" title="Size" width="200"></vxe-table-column>
				<vxe-table-column field="date" title="Date" width="200" sortable></vxe-table-column>
				<vxe-table-column field="shenhe" title="审核" width="200">
					<template v-slot="{ row }">
						<div>
							<vxe-checkbox :value="row.shenhe == '已审核'" @change="checkk($event,row)">{{row.shenhe}}</vxe-checkbox>
						</div>
					</template>
				</vxe-table-column>
				<vxe-table-column field="type" title="操作" width="200" show-overflow>
					<template v-slot="{ row }">
						<vxe-button type="text"  class="vxe-icon--edit-outline" @click="aaa(row)"></vxe-button>
					</template>
				</vxe-table-column>
			</vxe-table>
		
		</div>
		<!-- 分页 -->
		<template>
		  <div class="block">
		    <!-- <span class="demonstration">分页</span> -->
		    <el-pagination
		      @size-change="handleSizeChange"
		      @current-change="handleCurrentChange"
		      :current-page="currentPage"
		      :page-sizes="[5, 10, 20]"
		      :page-size="pageSize"
		      layout="total, sizes, prev, pager, next, jumper"
		      :total="tableData.length" 
					>
		    </el-pagination>
		  </div>
		</template>
  </div>
</template>

<script>
	import XEUtils from  'xe-utils'
	
export default {
  name: 'HelloWorld',
  data () {
    return {
       tableData: [
					{ id: 10001, name: 'Test1', sex: '0', size: 28, shenhe: '未审核' },
					{ id: 10002, name: 'Test2', sex: '1', size: 22, shenhe: '未审核' },
					{ id: 10003, name: 'Test3', sex: '0', size: 32, shenhe: '未审核' },
					{ id: 10004, name: 'Test4', sex: '1', size: 23, shenhe: '未审核' },
					{ id: 10005, name: 'Test5', sex: '0', size: 30, shenhe: '未审核' },
					{ id: 10006, name: 'Test6', sex: '1', size: 21, shenhe: '未审核' },
					{ id: 10007, name: 'Test7', sex: '0', size: 29, shenhe: '未审核' },
					{ id: 10008, name: 'Test8', sex: '1', size: 35, shenhe: '未审核' }
				],
				currentPage: 1,
				pageSize: 5,
				value1: false
    }
  },
	methods:{
		selectWj (opts) {
			this.$refs.xTable.readFile(opts).then(params => {
				const { files } = params
				const records = Array.from(files).map(file => {
					let ns = file.name.split('.')
					let name = ns.slice(0, ns.length - 1).join('')
					let typ = ns[ns.length - 1]
					return {
						name: name,
						size: file.size,
						typ: typ,
						date: XEUtils.toDateString(new Date()),
						shenhe: '未审核'
					}
				})
				this.$refs.xTable.insert(records)
			})
		},
		// 审核
		checkk($event,row){
			console.log($event.checked)
			console.log(row.shenhe)
			if($event.checked == true){
				row.shenhe = '已审核'
			}else{
				row.shenhe = '未审核'
			}
		},
		aaa(val){
			console.log(val.shenhe)
			val.shenhe = '已审核'
		},
		// 新增
		insertEvent () {
		  let record = {
		    Sex: 1,
				shenhe: '未审核'
		  }
		  this.$refs.xTable.insert(record).then(({ row }) => this.$refs.xTable.setActiveCell(row, 'amount'))
			console.log(111)
			console.log(this.$refs.xTable)
		},
		// 删除
		removeEvent () {
		  this.$refs.xTable.removeCheckboxRow()
			// this.tableData.length = this.$refs.xTable.tableData.length
		  // this.tableData.splice(0,1)
		},
		// 分页
		handleSizeChange(val) {
			console.log(`每页 ${val} 条`);
		},
		handleCurrentChange(val) {
			console.log(`当前页: ${val}`);
			this.currentPage = val
		},
	}
}
</script>

<style scoped>
	.block{text-align: center; margin-top: 10px;}
	/* .clored{color: red!important; } */
  /deep/.vxe-checkbox>input:checked+.vxe-checkbox--icon+.vxe-checkbox--label{
		color: red;
	}
</style>
