<template>
    <div id="wrap">
			<div id="top_content">
				<div id="header">
					<div id="rightheader">
						<p>
							2009/11/20
							<el-button type="danger" @click="logout">安全退出</el-button>
						</p>
					</div>
					<div id="topheader">
						<h1 id="title">
							<a href="#">main</a>
						</h1>
					</div>
					<div id="navigation">
					</div>
				</div>
				<div id="content">
					<p id="whereami">
					</p>
					<h1>
             欢迎{{user_msg}}访问员工管理首页
					</h1>
					<table class="table">
						<tr class="table_header">
							<td>
								ID
							</td>
							<td>
								Name
							</td>
							<td>
								Photo
							</td>
							<td>
								Salary
							</td>
							<td>
								Age
							</td>
							<td>
								Operation
							</td>
						</tr>
						<tr v-for="emp in emp_list" :key="emp.id" :class="index%2==0?'row1':'row2'">
							<td>
								{{emp.id}}
							</td>
							<td>
								{{emp.emp_name}}
							</td>
							<td>
								<img :src="emp.img" style="height: 60px;">
							</td>
							<td>
								{{emp.salary}}
							</td>
							<td>
								{{emp.check_age}}
							</td>
							<td>
								<a href="javascript:;" @click="delEmp(emp.id)">删除员工</a>&nbsp;<router-link :to="'/update/'+emp.id">修改员工</router-link>
							</td>
						</tr>
					</table>
					<p>
						<router-link to="/add"><el-button type="success">添加员工</el-button></router-link>
					</p>
				</div>
			</div>
			<div id="footer">
				<div id="footer_bg">
				ABC@126.com
				</div>
			</div>
		</div>
</template>

<script>
    export default {
        name: "Index",
        data(){
          return{
            user_msg:"",
            emp_list:[],
          }
        },
        methods:{
          findAllEmp() {
            this.$axios({
              url: " http://127.0.0.1:8000/app/emp/",
              method: "get",
            }).then(res => {
              console.log(res.data.results);
              this.emp_list = res.data.results
            }).catch(error => {
              this.$message.error("查询出错了")
            })
          },
          delEmp(id){
            if (confirm("是否删除?")) {
              this.$axios({
                url: " http://127.0.0.1:8000/app/emp/" + id+"/",
                method: "delete",
              }).then(res => {
                console.log(res.data);
                this.$message("删除成功");
                this.findAllEmp()

              }).catch(error => {
                this.$message.error("删除失败")
              })
            }
          },
          logout(){
            this.$router.push('/login')
          }
        },
        created() {
          let username = sessionStorage.getItem("user");
          this.user_msg = username;
          if (username){
            this.$message("访问首页成功")

          }else{
            this.$message.error("您还没有登录，请登录");
            this.$router.push("/login")
          }
          this.findAllEmp();
          this.delEmp(id);
        }
    }
</script>

<style scoped>

</style>
