<template>
    <div id="wrap">
			<div id="top_content">
					<div id="header">
						<div id="rightheader">
							<p>
								2009/11/20
								<br />
							</p>
						</div>
						<div id="topheader">
							<h1 id="title">
								<a href="#">Main</a>
							</h1>
						</div>
						<div id="navigation">
						</div>
					</div>
				<div id="content">
					<p id="whereami">
					</p>
					<h1>
						修改员工
					</h1>
					<form action="emplist.html" method="post">
						<table cellpadding="0" cellspacing="0" border="0"
							class="form_table">
							<tr>
								<td valign="middle" align="right">
									id:
								</td>
								<td valign="middle" align="left">
									{{emp_id}}
								</td>
							</tr>
							<tr>
								<td valign="middle" align="right">
									name:
								</td>
								<td valign="middle" align="left">
									<input type="text" class="inputgri" name="name" v-model="emp_name"/>
								</td>
							</tr>
							<tr>
								<td valign="middle" align="right">
									photo:
								</td>
								<td valign="middle" align="left">
									<input type="file" name="photo" ref="photo"/>
								</td>
							</tr>
							<tr>
								<td valign="middle" align="right">
									salary:
								</td>
								<td valign="middle" align="left">
									<input type="text" class="inputgri" name="salary" v-model="salary"/>
								</td>
							</tr>
							<tr>
								<td valign="middle" align="right">
									age:
								</td>
								<td valign="middle" align="left">
									<input type="text" class="inputgri" name="age" v-model="age"/>
								</td>
							</tr>
						</table>
						<p>
							<el-button type="success" @click="updateEmp">修改</el-button>
						</p>
					</form>
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
        name: "Update",
        data(){
          return{
            emp_id: this.$route.params.id,
            emp_name:"",
            salary:"",
            age:"",
          }
        },
        methods:{
          get_emp(){
            this.$axios({
              url: "http://127.0.0.1:8000/app/emp/"+this.emp_id+'/',
              method: "get",
            }).then(res => {
              let emp = res.data.results;
              this.emp_name = emp.emp_name;
              this.age = emp.age;
              this.salary = emp.salary;
            }).catch(error => {
              this.$message.error("查询出错了")
            })

          },
          updateEmp(){
              let formData = new FormData();
              formData.append("emp_name", this.emp_name);
              formData.append("salary", this.salary);
              formData.append("age", this.age);
              let img_file = this.$refs.photo.files[0];
              if (img_file){
                formData.append("img",img_file);
              }
              this.$axios({
                  url: "http://127.0.0.1:8000/app/emp/"+this.emp_id+'/',
                  method: "patch",
                  data: formData,
                  headers:{
                      'content-type': 'multipart/form-data'
                  },
              }).then(res => {
                  console.log(res.data);
                  this.$router.push("/index");
              }).catch(error => {
                  this.$message.error("更新失败")
              })
          }
        },
        created() {
          this.get_emp();
        }

    }
</script>

<style scoped>

</style>
