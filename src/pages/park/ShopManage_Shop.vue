<template>
    <div>
    	<section>
        <common-table
	                :queryapi="queryapi"
	                :addapi="addapi"
	                :editapi="editapi"
	                :delapi="delapi"
	                :tableheight="tableheight"
	                :fieldsstr="fieldsstr"
	                :tableitems="tableitems"
	                :btswidth="btswidth"
	                :hide-export="hideExport"
	                :searchtitle="searchtitle"
	                :addtitle="addtitle"
	                :addFormRules="addFormRules"
	                :editFormRules="editFormRules"
	                :showdelete="showdelete"
	                :showresetpwd="showresetpwd"
	                :showmRefill="showmRefill"
	                :showModifyCarNumber="showModifyCarNumber"
	                :hideOptions="hideOptions"
	                :hideSearch="hideSearch"
	                :showEdit="showEdit"
	                :showsetting="showsetting"
	
	                ref="bolinkuniontable"
	        ></common-table>
	    </section>
	    <!--重置密码-->
        <el-dialog
                title="重置密码"
                v-model="resetPwdVisible"
                size="tiny">
            <el-form ref="form" label-width="120px" style="margin-bottom:-30px">
                <el-form-item label="请输入新密码">
                    <el-input v-model="pwd1" style="width:90%"></el-input>
                </el-form-item>
                <el-form-item label="再次输入密码">
                    <el-input v-model="pwd2" style="width:90%"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
				<el-button @click="resetPwdVisible = false" size="small">取 消</el-button>
				<el-button type="primary" size="small" @click="resetPwd" :loading="resetloading">确 定</el-button>
			</span>
        </el-dialog>
        <!--注册员工-->
        <el-dialog
                title="添加员工"
                v-model="regUserVisible"
                size="tiny">
            <el-form ref="form" label-width="120px" style="margin-bottom:-30px">
            	<el-form-item label="编号" v-if="showInput">
                    <el-input :disabled="true" v-model="user.id" style="width:90%"></el-input>
               </el-form-item>             
                <el-form-item label="姓名">
                    <el-input v-model="user.nickname" style="width:90%"></el-input>
                </el-form-item>
                <el-form-item label="登陆账号" v-if="showInput">
                    <el-input v-model="user.strid" style="width:90%"></el-input>
                </el-form-item>
                <el-form-item label="电话">
                    <el-input v-model="user.phone" style="width:90%"></el-input>
                </el-form-item>
                <el-form-item label="手机">
                    <el-input v-model="user.mobile" style="width:90%"></el-input>
                </el-form-item>  
                <el-form-item label="角色">
                   <el-select v-model="user.auth_flag" placeholder="请选择">
				    <el-option
				      v-for="item in options"
				      :key="item.value"
				      :label="item.label"
				      :value="item.value">
				    </el-option>
				  </el-select>
                </el-form-item>
           
                <el-form-item   label="创建时间" v-if="showInput">
                    <el-input :disabled="true" v-model="user.reg_time" style="width:90%"></el-input>
                </el-form-item>
                <el-form-item label="最近登陆时间" v-if="showInput">
                    <el-input :disabled="true" v-model="user.logon_time" style="width:90%"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
				<el-button @click="regUserVisible = false" size="small">取 消</el-button>
				<el-button type="primary" size="small" @click="saveUser" >保存</el-button>
			</span>
        </el-dialog>
    	<!--员工管理-->
			<el-dialog
			  title="商户设置-员工管理"
			  :visible.sync="employeeVisible"
			  width="10%"
			  :before-close="handleClose">
			  
			  <div>
			  		<el-button type="primary" size="small" @click="regUser">注册员工</el-button>
			  </div>		  
			  <template>
				  <el-table
				    :data="employeeData"
				    style="width: 100%"
				    max-height="250">
				    <el-table-column
				      fixed="left"
				      label="操作"
				      width="180">
				      <template slot-scope="scope">
				        <el-button
				          @click.native.prevent="handleEdit(scope.row)"
				          type="text"
				          size="small">
				    		       编辑
				        </el-button>
				        <el-button
				          @click.native.prevent="deleteRow(scope.row)"
				          type="text"
				          size="small"
				          style="color:red">
				        	     删除
				        </el-button>
				        <el-button
				          @click.native.prevent="resetPassword(scope.row)"
				          type="text"
				          size="small">
				         	    修改密码
				        </el-button>
				      </template>
				    </el-table-column>
				    <el-table-column
				     <el-table-column
				      type="index" 
				      label="索引"
				      width="100"
				      :index="indexMethod">
				    </el-table-column>
				    <el-table-column
				      prop="id"
				      label="编号"
				      width="120">
				    </el-table-column>
				    <el-table-column
				      prop="nickname"
				      label="姓名"
				      width="120">
				    </el-table-column>
				    <el-table-column
				      prop="strid"
				      label="登陆账号"
				      width="120">
				    </el-table-column>
				    <el-table-column
				      prop="phone"
				      label="电话"
				      width="120">
				    </el-table-column>
				    <el-table-column
				      prop="mobile"
				      label="手机"
				      width="150">
				    </el-table-column>
				    <el-table-column
				     :formatter="strFormat"
				      prop="auth_flag"
				      label="角色"
				      width="120">
				    </el-table-column>
				    
				    <el-table-column
				      :formatter="dateFormat"
				      prop="reg_time"
				      label="创建时间"
				      width="180">
				    </el-table-column>
				    <el-table-column
				      prop="logon_time"
				      label="最近登陆时间"
				      width="180">
				    </el-table-column>
				    
			  </el-table>
			</template>	
			<!--简单分页工具条-->
			<el-pagination
			  small
			  layout="prev, pager, next"
      		  @current-change="handleCurrentChange"
			  :total="total">
			</el-pagination>
		</el-dialog>
		<!--renewDialog-->
		<el-dialog
		  title="减免券购买(时长)"
		  :visible.sync="renewVisible"
		  width="10%"
		  :before-close="handleClose">
		  <form name="renewForm" style="font-size: 2em;">
		  	<el-row>
			  	  <el-col :span="5"><div class="grid-content bg-purple">&nbsp;</div></el-col>
				  <el-col :span="5"><div  class="grid-content bg-purple">减免小时(时):</div></el-col>
				  <el-col :span="6"><div class="grid-content bg-purple-light"><el-input v-model="ticket_time" placeholder="请输入内容"></el-input></div></el-col>
				  <el-col :span="6"><div class="grid-content bg-purple-light">(每小时{{discount_money}}元)</div></el-col>
				  <el-col :span="2"><div class="grid-content bg-purple-light">&nbsp;</div></el-col>
			  </el-row>
			  <input type="hidden" name="shop_id" v_model="id" />
			   <el-row>
			  	  <el-col :span="5"><div class="grid-content bg-purple">&nbsp;</div></el-col>
				  <el-col :span="5"><div class="grid-content bg-purple">应收金额(元):</div></el-col>
				  <el-col :span="6"><div class="grid-content bg-purple-light"><el-input placeholder="" v-model="totalMoney" :disabled="true"> </el-input></div></el-col>
				  <el-col :span="6"><div class="grid-content bg-purple-light">&nbsp;</div></el-col>
				  <el-col :span="2"><div class="grid-content bg-purple-light">&nbsp;</div></el-col>
			  </el-row>
			   <el-row>
			  	  <el-col :span="5"><div class="grid-content bg-purple">&nbsp;</div></el-col>
				  <el-col :span="5"><div class="grid-content bg-purple">当前折扣(%):</div></el-col>
				  <el-col :span="6"><div class="grid-content bg-purple-light"><el-input placeholder=""  v-model="discount_percent" :disabled="true"> </el-input></div></el-col>
				  <el-col :span="6"><div class="grid-content bg-purple-light">&nbsp;</div></el-col>
				  <el-col :span="2"><div class="grid-content bg-purple-light">&nbsp;</div></el-col>
			  </el-row>
			   <el-row>
			  	  <el-col :span="5"><div class="grid-content bg-purple">&nbsp;</div></el-col>
				  <el-col :span="5"><div class="grid-content bg-purple">实收金额(元):</div></el-col>
				  <el-col :span="6"><div class="grid-content bg-purple-light"><el-input placeholder="" name="addmoney" v-model="addmoney" :disabled="true"> </el-input></div></el-col>
				  <el-col :span="6"><div class="grid-content bg-purple-light">&nbsp;</div></el-col>
				  <el-col :span="2"><div class="grid-content bg-purple-light">&nbsp;</div></el-col>
			  </el-row>
		  </form>
		  		  
		  <span slot="footer" class="dialog-footer">
		    <el-button type="primary" @click="renewSub">确 定</el-button>
		  </span>
		</el-dialog>
		
		<!--删除提示框-->
        <el-dialog
                title="提示"
                v-model="delVisible"
                size="tiny"
                custom-class="deleteTip">
            <div class="el-message-box__status el-icon-warning"></div>
            <br/>
            <div style="margin-left:50px;vertical-align:middle;">确定删除吗?此操作不可恢复!</div>
            <span slot="footer" class="dialog-footer">
				<el-button @click="delVisible = false" size="small">取 消</el-button>
				<el-button type="primary" @click="handledelete" size="small">确 定</el-button>
			</span>
        </el-dialog>
		
    </div>
    
</template>


<script>

    import {
        path,
        checkURL,
        checkUpload,
        checkNumber,
        percision,
        parktypelist,
        distinctslist,
        checkCityInfo,
        provincelist,
        checkPass,
        centerpayset,
        singleDoubleLimit,
        ticketType
    } from '../../api/api';
    import util from '../../common/js/util'
    import common from '../../common/js/common'
    import CommonTable from '../../components/CommonTable'
    import connect from '../../common/js/connector'

    export default {
        components: {
            CommonTable
        },
        data() {
            return {
            	page:1,
            	total:0,
            	showInput:false,
            	user:{},
            	delVisible:false,
            	rowid:0,
            	resetPwdVisible:false,
            	regUserVisible:false,
            	discount_money:1,
            	addmoney:0,
            	discount_percent:0,
            	totalMoney:0,
            	renewVisible:false,
            	ticket_time:0,
            	employeeVisible:false,
            	showEdit:true,
            	showsetting:true,
            	hideSearch:true,
                loading: false,
                showresetpwd: false,
                hideExport: true,
                tableheight: '',
                hideOptions: false,
				hideAdd:false,
                showdelete: true,
                showEdit: true,
                showmRefill: true,
                queryapi: '/shop/quickquery',
                addapi: '/shop/create',
                editapi: '/shop/create',
                delapi: '/shop/delete',
                parkid: '',
                btswidth: '180',
                fieldsstr: 'id__name__address__create_time__mobile__validite_time__ticket_money__ticket_type__default_limit__validite_time',
                tableitems:  [{
                        hasSubs: false, subs: [
                            {
                                label: '编号',
                                prop: 'id',
                                width: '123',
                                type: 'str',
                                editable: false,
                                searchable: true,
                                addable: false,
                                unsortable: true,
                                align: 'center',
                            },
                        ]
                    },
                    {

                        hasSubs: false,
                        subs: [{
                            label: '商户名称',
                            prop: 'name',
                            width: '123',
                            type: 'str',
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    }, {

                        hasSubs: false,
                        subs: [{
                            label: '地址',
                            prop: 'address',
                            width: '180',
                            type: 'str',
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{

                        hasSubs: false,
                        subs: [{
                            label: '创建时间',
                            prop: 'create_time',
                            width: '180',
                            type: 'date',
                            editable: false,
                            searchable: true,
                            addable: false,
                            unsortable: true,
                            align: 'center',
                            format:function (row) {
                                return common.dateformat(row.create_time)
                            }
                        }]
                    },{

                        hasSubs: false,
                        subs: [{
                            label: '手机',
                            prop: 'mobile',
                            width: '180',
                            type: 'str',
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center',
                        }]
                    },  {

                        hasSubs: false,
                        subs: [{
                            label: '优惠类型',
                            prop: 'ticket_type',
                            width: '100',
                            type: 'selection',
                            selectlist:ticketType,
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center',
                            format:function (row) {
                                if(row.ticket_type==1){
                                	return "时长减免"
                                }else{
                                	return "金额减免";
                                }
                            }
                        }]
                    }, {

                        hasSubs: false,
                        subs: [{
                            label: '默认显示额度',
                            prop: 'default_limit',
                            width: '180',
                            type: 'str',
                            editable: true,
                            searchable: false,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{

                        hasSubs: false,
                        subs: [{
                            label: '商户折扣/%',
                            prop: 'discount_percent',
                            width: '123',
                            type: 'str',
                            editable: true,
                            searchable: true,
                            addable: true,
                            unsortable: true,
                            align: 'center',
                        }]
                    }  
                    
                    
                    ,{

                        hasSubs: false,
                        subs: [{
                            label: '每小时/元',
                            prop: 'discount_money',
                            width: '100',
                            type: 'str',
                            editable: true,
                            searchable: false,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    },{

                        hasSubs: false,
                        subs: [{
                            label: '有效期/小时',
                            prop: 'validite_time',
                            width: '100',
                            type: 'str',
                            editable: true,
                            searchable: false,
                            addable: true,
                            unsortable: true,
                            align: 'center'
                        }]
                    }

                ],
                addtitle: '添加商户',
                employeeData:[],
                shop_id:'',
                pwd1:'',
                pwd2:'',
                options: [{
		          value: '14',
		          label: '负责人'
		        }, {
		          value: '15',
		          label: '工作人员'
		        }]
	        }
        },
        methods :{
        	handleCurrentChange(val){
        		this.page=val;
        		this.loadData();
        	},
            renewSub(){
            	var vm = this;
                var api = this.editapi;
                var user = sessionStorage.getItem('user');
            	user = JSON.parse(user)

                var formObj = { }
                formObj.shop_id=this.id;
    			formObj.ticket_time=this.ticket_time;
    			formObj.addmoney=this.addmoney;
				formObj.operator=user.userid;
				formObj.parkid=user.parkid;
                //发起修改位置
                vm.maploading = true;
                vm.$post(path + '/shop/addmoney', formObj, function (ret) {
                	
                    if (ret.validate != 'undefined' && ret.validate == '1') {
                        //过期.重新登录
                        setTimeout(() => {
                            vm.alertInfo('登录过期,请重新登录!')
                        }, 100)
                    } else if (ret.validate != 'undefined' && ret.validate == '2') {
                        //令牌无效.重新登录
                        setTimeout(() => {
                            vm.alertInfo('登录异常,请重新登录!')
                        }, 100)
                    } else {
                        if (ret.state === 1) {
                            //更新成功
                            vm.$message({
                                message: '续费成功!',
                                type: 'success',
                                duration: 600,
                            });
                            
                        } else {
                            //更新失败
                            vm.$message({
                                message: '续费失败!',
                                type: 'error',
                                duration: 600
                            });
                        }
                        vm.renewVisible = false
                    }
                }, "json")
            },
            resetPassword(row){
            	this.rowid = row.id
                this.pwd1 = ''
                this.pwd2 = ''
                //显示充值密码对话框
            	this.resetPwdVisible=true;
            },
            loadData(){
            	var vm = this;
                var api = this.editapi;
                var user = sessionStorage.getItem('user');
            	user = JSON.parse(user)

                var formObj = { }
                formObj.shop_id=this.shop_id;
  				formObj.page=this.page;
                //发起修改位置
                vm.maploading = true;
                vm.$post(path + '/shopmember/quickquery', formObj, function (ret) {             	
                    	vm.employeeData=ret.rows;
                    	vm.total=ret.total;
                    	vm.page=ret.page;
                }, "json")
            },
            saveUser(userId){
            	
            	var vm = this;
                var user = sessionStorage.getItem('user');
            	user = JSON.parse(user)
                var formObj = {};
                formObj.shop_id=this.shop_id;
                formObj.comid=user.comid;
    			formObj.nickname=this.user.nickname;
    			formObj.phone = this.user.phone;
    			formObj.mobile=this.user.mobile;
    			formObj.auth_flag=this.user.auth_flag;
				formObj.userId=this.user.id;
                //发起修改位置
                vm.$post(path + '/shopmember/create', formObj, function (ret) {
                	
                    if (ret.validate != 'undefined' && ret.validate == '1') {
                        //过期.重新登录
                        setTimeout(() => {
                            vm.alertInfo('登录过期,请重新登录!')
                        }, 100)
                    } else if (ret.validate != 'undefined' && ret.validate == '2') {
                        //令牌无效.重新登录
                        setTimeout(() => {
                            vm.alertInfo('登录异常,请重新登录!')
                        }, 100)
                    } else {
                        if (ret.state === 1) {
                            //更新成功
                            vm.$message({
                                message: '操作成功!',
                                type: 'success',
                                duration: 600,
                            });
                             vm.loadData();
                            
                        } else {
                            //更新失败
                            vm.$message({
                                message: '操作失败!',
                                type: 'error',
                                duration: 600
                            });
                        }
                        vm.regUserVisible = false
                    }
                }, "json");
                
            },
            deleteRow(row){           	
                this.rowid = row.id
                this.delVisible = true            
            },
            //删除
            handledelete() {
                var vm = this;
                var dform = {'id': this.rowid}
                //发送请求,删除id为row.id的数据
                vm.$post(path +'/shopmember/delete', dform, function (ret) {
                    if (ret.validate != 'undefined' && ret.validate == '1') {
                        //过期.重新登录
                        setTimeout(() => {
                            vm.alertInfo('登录过期,请重新登录!')
                        }, 100)
                    } else if (ret.validate != 'undefined' && ret.validate == '2') {
                        //令牌无效.重新登录
                        setTimeout(() => {
                            vm.alertInfo('登录异常,请重新登录!')
                        }, 100)
                    } else {
                        console.log(ret)
                        if (ret > 0 || ret.state == 1) {
                            vm.loadData();
                            //删除成功
                            vm.$message({
                                message: '删除成功!',
                                type: 'success',
                                duration: 600
                            });
                            vm.delVisible = false
                            
                        } else {
                            //更新失败
                            vm.$message({
                                message: "更新失败",
                                type: 'error',
                                duration: 1200
                            });
                        }
                    }

                }, "json")
            },
            resetPwd() {
                var vm = this
                if (this.pwd1 == '' || this.pwd2 == '') {
                    this.$message.error('密码不能为空!');
                    return;
                }
                if (!(/^(\w){6,12}$/.test(this.pwd1)) || !(/^(\w){6,12}$/.test(this.pwd2))) {
                    this.$message.error('密码为6-12位字母,数字或下划线!');
                    return
                }
                if (this.pwd1 != this.pwd2) {
                    this.$message.error('两次输入密码不一致!');
                    return
                }
                vm.$post(path + "/shopmember/editpass", {
                    'newpass': this.pwd1,
                    'confirmpass': this.pwd2,
                    'id': this.rowid,
                }, function (ret) {
                    if (ret.validate != 'undefined' && ret.validate == '1') {
                        //过期.重新登录
                        setTimeout(() => {
                            vm.alertInfo('登录过期,请重新登录!')
                        }, 100)
                    } else if (ret.validate != 'undefined' && ret.validate == '2') {
                        //令牌无效.重新登录
                        setTimeout(() => {
                            vm.alertInfo('登录异常,请重新登录!')
                        }, 100)
                    } else {
                        if (ret > 0 || ret.state == 1) {
                            //更新成功
                            vm.loadData()
                            vm.$message({
                                message: '重置成功!',
                                type: 'success',
                                duration: 1500
                            });
                            vm.resetPwdVisible = false
                        } else {
                            //更新失败
                            vm.$message({
                                message: '更新失败!',
                                type: 'error',
                                duration: 2000
                            });
                        }
                    }
                }, "json")

            },
            handleEdit(row){
            	this.user=row;
            	this.showInput=true;
            	this.regUserVisible=true;
            	
            },
            regUser(){
            	this.user={};
            	this.showInput=false;
            	this.regUserVisible=true;
            },
            dateFormat:function(row, column) {  
          	  var date = row[column.property];  
	          if (date == undefined) {  
	             return "";  
	         }          
	         return common.dateformat(row.reg_time);
	        },  
	        strFormat(row, column){
	        	if(row.auth_flag==14){
	        		return "负责人"
	        	}else{
	        		return "工作人员"
	        	}
	        },
	        prev(){
	        	alert("aaa");
	        }
        }
        ,
        mounted() {
            window.onresize = () => {
                this.tableheight = common.gwh() - 135;
            }
            this.tableheight = common.gwh() - 135;
        },
        activated() {
            console.log('active')
            console.log(distinctslist)
            window.onresize = () => {
                this.tableheight = common.gwh() - 135;
            }
            this.tableheight = common.gwh() - 135;
            this.$refs['bolinkuniontable'].$refs['search'].resetSearch()
            this.$refs['bolinkuniontable'].getTableData({})
            //所有厂商,所有服务商
            this.axios.all([common.getServerList(), common.getUnionList(), common.getBankInfo(), common.getBaPayUnionList()])
        },
        created(){
        	var vm = this;
        	connect.$on('setting',function(row){
        		vm.shop_id=row.id;

        		//请求员工数据
        		var user = sessionStorage.getItem('user')
                user = JSON.parse(user)
                vm.loadData();
        		//设置
				vm.employeeVisible=true;
			})
        	connect.$on('renew',function(row){
					//续费
	                vm.ticket_time=0;
		    		vm.id=row.id;
		    		vm.ticket_money=row.ticket_money;
		    		vm.discount_percent=row.discount_percent;
		    		vm.discount_money=row.discount_money;
					vm.renewVisible=true;
			})
        },
        watch:{
        	     	
	     	ticket_time(curVal,oldVal){
	     		
	　　　　　　　　　　if(!isNaN(curVal)){
						this.totalMoney=curVal*this.discount_money;
						this.addmoney=curVal*this.discount_money*this.discount_percent/100;
					}else{
						this.totalMoney=0;
						this.addmoney=0;
						this.curVal=0;
					}
	　　　　　}
	     }
    }

</script>