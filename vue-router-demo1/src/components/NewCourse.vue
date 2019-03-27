<template>
	<div class="container">
		<input type="text" placeholder="请输入班课名称" v-model="course.courseName" class="input-box" />
		<input type="text" placeholder="请输入班级" v-model="course.courseClass" class="input-box" />
		<p class="title">设置班课封面</p >
		<div class="preview" @click="handleClick()">
			<img :src="course.cover" class="course-cover" v-if="!show" />
			<div class="icon-plus">
				<img src="../assets/plus.png"  v-if="show" />
			</div>
				
			
			<input type="file" @change="getFile($event)" style="display: none;" id="coverFile" />
		</div>
		<div class="btn">
			<button @click="addCourse(course)" class="btn1">确定</button>
			<button class="btn2">取消</button>
		</div>

	</div>
</template>

<script>
	export default {
		name: 'NewCourse',
		data() {
			return {
				loginUserId: 1,
				course: {
					courseName: '',
					courseClass: '',
					cover: '',
				},
				file: '',
				show: true
			};
		},
		methods: {
			addCourse: function(course) {
				var _this = this;
				this.$http({
					method: 'post',
					url: 'http://localhost:8080/api/course',
					data: {
						userId: _this.loginUserId,
						courseName: course.courseName,
						courseClass: course.courseClass,
						cover: course.cover,
						finished: 0
					}
				}).then(function() {
					alert('新增班课成功');
					_this.$router.push('/');
				});
			},
			//点击图片预览区，即模拟点击文件选择组件
			handleClick: function() {
				document.getElementById('coverFile').click();
			},
			//图片预览
			getFile: function() {
				this.file = event.target.files[0];
				var windowURL = window.URL || window.webkitURL;
				this.course.cover = windowURL.createObjectURL(this.file);
				this.show = false;
			}

		}
	};
</script>
<style scoped>
	.container {
		display: flex;
		flex-direction: column;
		padding-top: 20px;
		padding-left: 100px;
		background-color: #fff;
		margin-top: 20px;
	}

	.input-box {
		width: 500px;
		height: 40px;
		margin-bottom: 40px;
		font-size: 14px;
	}

	.title {
		font-size: 13px;
		position: relative;
		top: -0.625rem;
	}

	.btn {
		display: flex;
	}

	.btn1 {
		margin-top: 20px;
		width: 120px;
		height: 40px;
		border: 2px solid rgb(0, 187, 221);
		background-color: #fff;
		border-radius: 5px;
		outline: none;
		color: rgb(0, 187, 221);
		font-size: 16px;
		cursor: pointer;
	}

	.btn2 {
		margin-left: 200px;
		width: 120px;
		height: 40px;
		margin-top: 20px;
		cursor: pointer;
		font-size: 16px;
		background-color: #fff;
		border: 2px solid #AAAAAA;
		outline: none;
	}

	.btn2:hover {
		color: #AAAAAA;
	}

	.preview {
		width: 150px;
		height: 150px;
		border: 2px dashed #aaa;
		cursor: pointer;
		display: flex;
		align-items: center;
		justify-content: center;
		margin-left: 20px;
	}

	.icon-plus {
		width: 60px;
		height: 60px;
	}

	.icon-plus img {
		width: 100%;
		height: 100%;
		/* margin-left: auto;
		margin-right: auto; */
	}

	.course-cover {
		width: 100%;
		height: 100%;
		margin-left: auto;
		margin-right: auto;
	}
</style>

