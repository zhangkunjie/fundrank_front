<template>
	<section>
		<!--工具条-->
		<el-col :span="24" class="toolbar" style="padding-bottom: 0px;">
			<el-form :inline="true" :model="filters">
        <el-form-item label="类型">
			    <el-select v-model="filters.category" value="(gp,hh,hh,zq,zs,bb,qdii,lof)" v-on:change="getUsers()"  style="width:80px">
				<el-option label="全部" value="'gp','hh','hh','zq','zs','bb','qdii','lof'"></el-option>
        <el-option label="股票" value="'gp'"></el-option>
				<el-option label="混合" value="'hh'"></el-option>
				<el-option label="债券" value="'zq'"></el-option>
				<el-option label="指数" value="'zs'"></el-option>
				<el-option label="保本" value="'bb'"></el-option>
				<el-option label="qdii" value="'qdii'"></el-option>
				<el-option label="lof" value="'lof'"></el-option>
			  </el-select>
		    </el-form-item>
        <el-form-item>
					<el-input v-model="filters.minOnemonth" placeholder="月最小" style="width:70px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.maxOnemonth" placeholder="月最大" style="width:70px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input  v-model="filters.minThreemonth" placeholder="3月最小" style="width:70px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.maxThreemonth" placeholder="3月最大" style="width:75px"></el-input>
				</el-form-item>
          <el-form-item>
					<el-input   v-model="filters.minSixmonth" placeholder="6月最小" style="width:75px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.maxSixmonth"  placeholder="6月最大" style="width:75px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.minOneyear"  placeholder="1年最小" style="width:75px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.maxOneyear" placeholder="1年最大" style="width:75px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.minTwoyear" placeholder="2年最小" style="width:75px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.maxTwoyear" placeholder="2年最大" style="width:75px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.minThreeyear" placeholder="3年最小" style="width:75px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.maxThreeyear" placeholder="3年最大" style="width:75px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.minThisyear" placeholder="今年最小" style="width:75px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.maxThisyear"  placeholder="今年最大" style="width:75px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input  v-model="filters.minSetup" placeholder="成立最小" style="width:75px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.maxSetup" placeholder="成立最大" style="width:75px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.minScore" placeholder="得分最小" style="width:75px"></el-input>
				</el-form-item>
        <el-form-item>
					<el-input v-model="filters.maxScore" placeholder="得分最大" style="width:75px"></el-input>
				</el-form-item>
				<el-form-item>
					<el-button type="primary" v-on:click="getUsers">查询</el-button>
				</el-form-item>
        <el-form-item>
					<el-button type="primary" @click="resetForm('filters')">重置</el-button>
				</el-form-item>
				<!-- <el-form-item>
					<el-button type="primary" @click="handleAdd">新增</el-button>
				</el-form-item> -->
				<el-form-item label="每页数量">
			    <el-select v-model="filters.pageSize" value="20" v-on:change="getUsers()"  style="width:70px">
				<el-option label="15" value="15"></el-option>
				<el-option label="30" value="30"></el-option>
				<el-option label="50" value="50"></el-option>
				<el-option label="100" value="100"></el-option>
			    </el-select>
		        </el-form-item>
			</el-form>
		</el-col>

		<!--列表-->
		<el-table :data="users" highlight-current-row v-loading="listLoading" @sort-change="sortChange" @selection-change="selsChange" style="width: 100%;">
			<el-table-column type="selection" width="55">
			</el-table-column>
			<el-table-column  prop="category" label="类别">
			</el-table-column>
			<el-table-column  prop="fundId" label="基金代码" >
			</el-table-column>
			<el-table-column prop="fundName" label="基金名称" >
			</el-table-column>
			<el-table-column prop="calDate" label="计算日期" >
			</el-table-column>
			<el-table-column prop="netAssetValue" label="单位净值"  sortable='custom'>
			</el-table-column>
			<el-table-column prop="accumulative" label="累计净值"  sortable>
			</el-table-column>
			<el-table-column prop="oneday" label="日增长率"  sortable>
			</el-table-column>
			<el-table-column prop="oneweek" label="1周"  sortable>
			</el-table-column>
			<el-table-column prop="onemonth" label="1月"  sortable>
			</el-table-column>
			<el-table-column prop="threemonth" label="3月"  sortable>
			</el-table-column>
			<el-table-column prop="sixmonth" label="6月"  sortable>
			</el-table-column>
			<el-table-column prop="oneyear" label="1年"  sortable>
			</el-table-column>
			<el-table-column prop="twoyear" label="2年"  sortable>
			</el-table-column>
			<el-table-column prop="threeyear" label="3年" sortable>
		    </el-table-column>
			<el-table-column prop="thisyear" label="今年"  sortable>
			</el-table-column>
			<el-table-column prop="setup" label="成立以来"  sortable>
			</el-table-column> 
      <el-table-column prop="score" label="得分"  sortable>
			</el-table-column> 
			<!-- <el-table-column label="操作" width="150">
				<template scope="scope">
					<el-button size="small" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
					<el-button type="danger" size="small" @click="handleDel(scope.$index, scope.row)">删除</el-button>
				</template>
			</el-table-column> -->
		</el-table>

		<!--工具条-->
		<el-col :span="24" class="toolbar">
			<el-button type="danger" @click="batchRemove" :disabled="this.sels.length===0">批量删除</el-button>
			<el-pagination layout="prev, pager, next" @current-change="handleCurrentChange" :page-size="20" :total="total" style="float:right;">
			</el-pagination>
		</el-col>

		<!--编辑界面-->
		<el-dialog title="编辑" v-model="editFormVisible" :close-on-click-modal="false">
			<el-form :model="editForm" label-width="80px" :rules="editFormRules" ref="editForm">
				<el-form-item label="姓名" prop="name">
					<el-input v-model="editForm.name" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="性别">
					<el-radio-group v-model="editForm.sex">
						<el-radio class="radio" :label="1">男</el-radio>
						<el-radio class="radio" :label="0">女</el-radio>
					</el-radio-group>
				</el-form-item>
				<el-form-item label="年龄">
					<el-input-number v-model="editForm.age" :min="0" :max="200"></el-input-number>
				</el-form-item>
				<el-form-item label="生日">
					<el-date-picker type="date" placeholder="选择日期" v-model="editForm.birth"></el-date-picker>
				</el-form-item>
				<el-form-item label="地址">
					<el-input type="textarea" v-model="editForm.addr"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click.native="editFormVisible = false">取消</el-button>
				<el-button type="primary" @click.native="editSubmit" :loading="editLoading">提交</el-button>
			</div>
		</el-dialog>

		<!--新增界面-->
		<el-dialog title="新增" v-model="addFormVisible" :close-on-click-modal="false">
			<el-form :model="addForm" label-width="80px" :rules="addFormRules" ref="addForm">
				<el-form-item label="姓名" prop="name">
					<el-input v-model="addForm.name" auto-complete="off"></el-input>
				</el-form-item>
				<el-form-item label="性别">
					<el-radio-group v-model="addForm.sex">
						<el-radio class="radio" :label="1">男</el-radio>
						<el-radio class="radio" :label="0">女</el-radio>
					</el-radio-group>
				</el-form-item>
				<el-form-item label="年龄">
					<el-input-number v-model="addForm.age" :min="0" :max="200"></el-input-number>
				</el-form-item>
				<el-form-item label="生日">
					<el-date-picker type="date" placeholder="选择日期" v-model="addForm.birth"></el-date-picker>
				</el-form-item>
				<el-form-item label="地址">
					<el-input type="textarea" v-model="addForm.addr"></el-input>
				</el-form-item>
			</el-form>
			<div slot="footer" class="dialog-footer">
				<el-button @click.native="addFormVisible = false">取消</el-button>
				<el-button type="primary" @click.native="addSubmit" :loading="addLoading">提交</el-button>
			</div>
		</el-dialog>
	</section>
</template>

<script>
import util from "../../common/js/util";
//import NProgress from 'nprogress'
import {
  getUserListPage,
  removeUser,
  batchRemoveUser,
  editUser,
  addUser
} from "../../api/api";

export default {
  data() {
    return {
      filters: {
        pageSize: 15,
        category: "'gp','hh','hh','zq','zs','bb','qdii','lof'"
      },
      users: [],
      total: 0,
      page: 1,
      listLoading: false,
      sels: [], //列表选中列

      editFormVisible: false, //编辑界面是否显示
      editLoading: false,
      editFormRules: {
        name: [{ required: true, message: "请输入姓名", trigger: "blur" }]
      },
      //编辑界面数据
      editForm: {
        id: 0,
        name: "",
        sex: -1,
        age: 0,
        birth: "",
        addr: ""
      },

      addFormVisible: false, //新增界面是否显示
      addLoading: false,
      addFormRules: {
        name: [{ required: true, message: "请输入姓名", trigger: "blur" }]
      },
      //新增界面数据
      addForm: {
        name: "",
        sex: -1,
        age: 0,
        birth: "",
        addr: ""
      }
    };
  },
  methods: {
    //性别显示转换
    formatSex: function(row, column) {
      return row.sex == 1 ? "男" : row.sex == 0 ? "女" : "未知";
    },
    handleCurrentChange(val) {
      this.page = val;
      this.getUsers();
    },
    //清空form
    resetForm:function(formName) {
        this.$refs[formName].resetFields();
      },
    //获取用户列表
    getUsers() {
      let para = {
        pageSize: this.filters.pageSize == null ? 15 : this.filters.pageSize,
        pageNo: this.page,
        category: this.filters.category,
        orderColumn: this.orderColumn,
        orderBy: this.orderBy,
        minOnemonth: this.filters.minOnemonth,
        maxOnemonth: this.filters.maxOnemonth,
        minThreemonth: this.filters.minThreemonth,
        maxThreemonth: this.filters.maxThreemonth,
        minSixmonth: this.filters.minSixmonth,
        maxSixmonth: this.filters.maxSixmonth,
        minOneyear: this.filters.minOneyear,
        maxOneyear: this.filters.maxOneyear,
        minTwoyear: this.filters.minTwoyear,
        maxTwoyear: this.filters.maxTwoyear,
        minThreeyear: this.filters.minThreeyear,
        maxThreeyear: this.filters.maxThreeyear,
        minThisyear: this.filters.minThisyear,
        maxThisyear: this.filters.maxThisyear,
        minSetup: this.filters.minSetup,
        maxSetup: this.filters.maxSetup,
        minScore: this.filters.minScore,
        maxScore: this.filters.maxScore
      };
      this.listLoading = true;
      //NProgress.start();
      getUserListPage(para).then(res => {
        //alert(para.orderByColumn);
        this.listLoading = false;
        this.total = res.data.pairs.dat.total;
        this.users = res.data.pairs.dat.list;
        //NProgress.done();
      });
    },
    //表格服务端排序
    sortChange: function(column, prop, order) {
      this.orderColumn = column.prop;
      this.orderBy = column.order == "ascending" ? "asc" : "desc";
      this.getUsers();
    },
    //删除
    handleDel: function(index, row) {
      this.$confirm("确认删除该记录吗?", "提示", {
        type: "warning"
      })
        .then(() => {
          this.listLoading = true;
          //NProgress.start();
          let para = { id: row.id };
          removeUser(para).then(res => {
            this.listLoading = false;
            //NProgress.done();
            this.$message({
              message: "删除成功",
              type: "success"
            });
            this.getUsers();
          });
        })
        .catch(() => {});
    },
    //显示编辑界面
    handleEdit: function(index, row) {
      this.editFormVisible = true;
      this.editForm = Object.assign({}, row);
    },
    //显示新增界面
    handleAdd: function() {
      this.addFormVisible = true;
      this.addForm = {
        name: "",
        sex: -1,
        age: 0,
        birth: "",
        addr: ""
      };
    },
    //编辑
    editSubmit: function() {
      this.$refs.editForm.validate(valid => {
        if (valid) {
          this.$confirm("确认提交吗？", "提示", {}).then(() => {
            this.editLoading = true;
            //NProgress.start();
            let para = Object.assign({}, this.editForm);
            para.birth =
              !para.birth || para.birth == ""
                ? ""
                : util.formatDate.format(new Date(para.birth), "yyyy-MM-dd");
            editUser(para).then(res => {
              this.editLoading = false;
              //NProgress.done();
              this.$message({
                message: "提交成功",
                type: "success"
              });
              this.$refs["editForm"].resetFields();
              this.editFormVisible = false;
              this.getUsers();
            });
          });
        }
      });
    },
    //新增
    addSubmit: function() {
      this.$refs.addForm.validate(valid => {
        if (valid) {
          this.$confirm("确认提交吗？", "提示", {}).then(() => {
            this.addLoading = true;
            //NProgress.start();
            let para = Object.assign({}, this.addForm);
            para.birth =
              !para.birth || para.birth == ""
                ? ""
                : util.formatDate.format(new Date(para.birth), "yyyy-MM-dd");
            addUser(para).then(res => {
              this.addLoading = false;
              //NProgress.done();
              this.$message({
                message: "提交成功",
                type: "success"
              });
              this.$refs["addForm"].resetFields();
              this.addFormVisible = false;
              this.getUsers();
            });
          });
        }
      });
    },
    selsChange: function(sels) {
      this.sels = sels;
    },
    //批量删除
    batchRemove: function() {
      var ids = this.sels.map(item => item.id).toString();
      this.$confirm("确认删除选中记录吗？", "提示", {
        type: "warning"
      })
        .then(() => {
          this.listLoading = true;
          //NProgress.start();
          let para = { ids: ids };
          batchRemoveUser(para).then(res => {
            this.listLoading = false;
            //NProgress.done();
            this.$message({
              message: "删除成功",
              type: "success"
            });
            this.getUsers();
          });
        })
        .catch(() => {});
    }
  },
  mounted() {
    this.getUsers();
  }
};
</script>

<style scoped>
</style>