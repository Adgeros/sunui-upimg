<template>
	<view>
		<view>
			<button type="primary" @tap="prievwImage">选择图片</button>
			<!-- 不会覆盖限制上传图片数量（也就是count参数生效），notli默认false -->
			<sunui-upimg :upImgConfig="upImgConfig" @onUpImg="upImgData" ref="uImage" />
			<button type="primary" @tap="getUpImgInfo">获取上传图片信息</button>

			<sunui-upbasic :upImgConfig="upImgBasic" @onUpImg="upBasicData"></sunui-upbasic>
			<button type="primary" @tap="getUpImgInfoBasic">获取上传Basic图片信息</button>


			<sunui-upoos :upImgConfig="upImgOos" @onUpImg="upOosData" @onImgDel="delImgInfo"></sunui-upoos>
			<button type="primary" @tap="getUpImgInfoOos">获取上传Oos图片信息</button>

			<sunui-upqiniu :upImgConfig="upImgQiniu" @onUpImg="upQiniuData"></sunui-upqiniu>
			<button type="primary" @tap="getUpImgInfoQiniu">获取上传Qiniu图片信息</button>
			<!-- <button type="primary" @tap="uImageTap">上传图片</button> -->

		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				basicArr: [],
				imgArr: [],
				oosArr: [],
				qiniuArr: [],
				// 七牛云相关配置
				upImgQiniu: {
					qiniuConfig: {
						region: 'SCN',
						uptokenURL: 'wuqiangxi',
						uptoken: 'Ycs3ADclVWZAWvqdocpkr2i3oEHtg_LSJSFOBl2E:JD3xxMz6rwNKiVS2bDnA8Bkvgd0=:eyJzY29wZSI6Ind1cWlhbmd4aSIsImRlYWRsaW5lIjoxNTU3MTMyNjE3LCJ1cEhvc3RzIjpbImh0dHA6XC9cL3VwLXoyLnFpbml1LmNvbSIsImh0dHA6XC9cL3VwbG9hZC16Mi5xaW5pdS5jb20iLCItSCB1cC16Mi5xaW5pdS5jb20gaHR0cDpcL1wvMTQuMTUyLjM3LjQiXX0=',
						domain: 'wpx.weijuyunke.cn',
						shouldUseQiniuFileName: false,
						fileHead: 'file',
						key: (new Date()).getTime()
					},
					// 是否开启notli(开启的话就是选择完直接上传，关闭的话当count满足数量时才上传)
					notli: false,
					// 图片数量
					count: 2,
					// 过滤图片容量大于或等于3M的图片(2019/4/11新增)
					maxsize: 1,
					// 相机来源([相机,相册],[相机])
					sourceType: true,
					// 是否压缩上传照片(仅小程序生效)
					sizeType: true,
					// 新增上传背景修改
					bgColor: '#0089FC',
					// 新增上传icon图标颜色修改
					iconColor: '#fff',
					// 上传文字描述(仅限四个字)
					text: '上传图片',
					// 删除图标定义背景颜色
					delIconColor: '',
					// 删除图标字体颜色
					delIconText: '',
					// 上传图标替换(+),是个http,https图片地址(https://www.playsort.cn/right.png)
					iconReplace: ''
				},
				// 阿里云oos相关配置
				upImgOos: {
					aliConfig: {
						// 阿里云oos上传key_secret(后端传)
						AccessKeySecret: 'zmOJcaqKJB5e4gqtLunHcNoMBTdDgp',
						// 阿里云oos上传key_id(后端传)
						OSSAccessKeyId: 'LTAIPcJL9J5OZr2G',
						// 阿里云oos上传目录(必须存在)
						oosDirectory: 'mifanimg',
						// 阿里云上传url
						url: 'http://4zlinkimgtest.oss-cn-beijing.aliyuncs.com/',
						// 是否阿里云oos,true启用;false使用basicConfig.url(也就是公司后端提供的接口)
					},
					// 是否开启notli(开启的话就是选择完直接上传，关闭的话当count满足数量时才上传)
					notli: false,
					// 图片数量
					count: 2,
					// 相机来源([相机,相册],[相机])
					sourceType: true,
					// 是否压缩上传照片(仅小程序生效)
					sizeType: true,
					// 新增上传背景修改
					bgColor: '#0089FC',
					// 新增上传icon图标颜色修改
					iconColor: '#fff',
					// 上传文字描述(仅限四个字)
					text: '上传图片',
					// 删除图标定义背景颜色
					delIconColor: '',
					// 删除图标字体颜色
					delIconText: '',
					// 上传图标替换(+),是个http,https图片地址(https://www.playsort.cn/right.png)
					iconReplace: ''
				},
				// 基础版配置
				upImgBasic: {
					// 后端图片接口地址
					basicConfig: {
						url: 'https://p.dns06.net.cn/index.php?m=Api&c=index&a=upload'
					},
					// 是否开启notli(开启的话就是选择完直接上传，关闭的话当count满足数量时才上传)
					notli: false,
					// 图片数量
					count: 2,
					// 相机来源([相机,相册],[相机])
					sourceType: true,
					// 是否压缩上传照片(仅小程序生效)
					sizeType: true,
					// 新增上传背景修改
					bgColor: '#0089FC',
					// 新增上传icon图标颜色修改
					iconColor: '#fff',
					// 上传文字描述(仅限四个字)
					text: '上传图片',
					// 删除图标定义背景颜色
					delIconColor: '',
					// 删除图标字体颜色
					delIconText: '',
					// 上传图标替换(+),是个http,https图片地址(https://www.playsort.cn/right.png)
					iconReplace: ''
				},
				// 集成版相关配置
				upImgConfig: {
					// 阿里云相关配置
					aliConfig: {
						// 阿里云oos上传key_secret(后端传)
						AccessKeySecret: 'zmOJcaqKJB5e4gqtLunHcNoMBTdDgp',
						// 阿里云oos上传key_id(后端传)
						OSSAccessKeyId: 'LTAIPcJL9J5OZr2G',
						// 阿里云oos上传目录(必须存在)
						oosDirectory: 'mifanimg',
						// 阿里云上传url
						url: 'http://4zlinkimgtest.oss-cn-beijing.aliyuncs.com/',
						// 是否阿里云oos,true启用;false使用basicConfig.url(也就是公司后端提供的接口)
						oos: true
					},
					// 七牛云相关配置
					qiniuConfig: {
						region: 'SCN', // 华北区
						uptokenURL: 'wuqiangxi',
						uptoken: 'Ycs3ADclVWZAWvqdocpkr2i3oEHtg_LSJSFOBl2E:2u_pMpb_4vNZBsYyrsCiwqu3Pvk=:eyJzY29wZSI6Ind1cWlhbmd4aSIsImRlYWRsaW5lIjoxNTU3MDMxMDgxLCJ1cEhvc3RzIjpbImh0dHA6XC9cL3VwLXoyLnFpbml1LmNvbSIsImh0dHA6XC9cL3VwbG9hZC16Mi5xaW5pdS5jb20iLCItSCB1cC16Mi5xaW5pdS5jb20gaHR0cDpcL1wvMTQuMTUyLjM3LjQiXX0=',
						domain: 'wpx.weijuyunke.cn',
						shouldUseQiniuFileName: false,
						fileHead: 'file',
						key: (new Date()).getTime()
					},
					// 后端接口相关配置
					basicConfig: {
						url: 'https://p.dns06.net.cn/index.php?m=Api&c=index&a=upload'
					},
					// 是否开启notli(开启的话就是选择完直接上传，关闭的话当count满足数量时才上传)
					notli: false,
					// 图片数量
					count: 2,
					// 相机来源([相机,相册],[相机])
					sourceType: true,
					// 是否压缩上传照片(仅小程序生效)
					sizeType: true,
					// 新增上传背景修改
					bgColor: '#0089FC',
					// 新增上传icon图标颜色修改
					iconColor: '#fff',
					// 上传文字描述(仅限四个字)
					text: '上传图片',
					// 删除图标定义背景颜色
					delIconColor: '',
					// 删除图标字体颜色
					delIconText: '',
					// 上传图标替换(+),是个http,https图片地址(https://www.playsort.cn/right.png)
					iconReplace: ''
				}
			};
		},
		methods: {
			// 删除图片
			async delImgInfo(e){
				console.log('你删除的图片地址为:',e);
			},
			// 图片预览
			async prievwImage(){
				uni.previewImage({
					urls: ['https://www.playsort.cn/qqqb.png']
				});
			},
			// 上传图片(2019/3/29新增) -> 手动上传(需要传入上传url,还需要搭配count参数使用!)
			// 			uImageTap() {
			// 				this.$refs.uImage.uploadimage('http://4zlinkimgtest.oss-cn-beijing.aliyuncs.com/');
			// 			},

			// 七牛云
			async upQiniuData(e) {
				// 上传图片数组
				let arrImg = [];
				for (let i = 0, len = e.length; i < len; i++) {
					try {
						// let reg = /([^\s]+(?=\.(jpg|png|gif))\.\2)/gi;
						if (e[i].path_server != "") {
							await arrImg.push(e[i].path_server.split(','));
						}
					} catch (err) {
						console.log('上传失败...');
					}
				}
				// 图片信息保存到data数组
				this.qiniuArr = arrImg;
				// 可以根据长度来判断图片是否上传成功. 2019/4/11新增
				if (arrImg.length == this.upImgQiniu.count) {
					uni.showToast({
						title: `上传成功`,
						icon: 'none'
					});
				}
			},

			// 阿里云
			async upOosData(e) {
				// 上传图片数组
				let arrImg = [];
				for (let i = 0, len = e.length; i < len; i++) {
					try {
						if (e[i].path_server != "") {
							await arrImg.push(e[i].path_server.split(','));
						}
					} catch (err) {
						console.log('上传失败...');
					}
				}
				// 图片信息保存到data数组
				this.oosArr = arrImg;

				// 可以根据长度来判断图片是否上传成功. 2019/4/11新增
				if (arrImg.length == this.upImgOos.count) {
					uni.showToast({
						title: `上传成功`,
						icon: 'none'
					});
				}
			},

			// 基础版
			async upBasicData(e) {
				// 上传图片数组
				let arrImg = [];
				for (let i = 0, len = e.length; i < len; i++) {
					try {
						if (e[i].path_server != "") {
							await arrImg.push(e[i].path_server.split(','));
						}
					} catch (err) {
						console.log('上传失败...');
					}
				}
				// 图片信息保存到data数组
				this.basicArr = arrImg;

				// 可以根据长度来判断图片是否上传成功. 2019/4/11新增
				if (arrImg.length == this.upImgBasic.count) {
					uni.showToast({
						title: `上传成功`,
						icon: 'none'
					});
				}
			},
			//获取上传图片返回来的数组(完整示例版)
			async upImgData(e) {
				// 上传图片数组
				let arrImg = [];
				for (let i = 0, len = e.length; i < len; i++) {
					try {
						if (e[i].path_server != "") {
							await arrImg.push(e[i].path_server.split(','));
						}
					} catch (err) {
						console.log('上传失败...');
					}
				}
				// 图片信息保存到data数组
				this.imgArr = arrImg;

				// 可以根据长度来判断图片是否上传成功. 2019/4/11新增
				if (arrImg.length == this.upImgConfig.count) {
					uni.showToast({
						title: `上传成功`,
						icon: 'none'
					});
				}
			},
			// 获取上传图片的所有信息(完整示例版)
			getUpImgInfo() {
				console.log('转成多维数组:', this.imgArr);
				console.log('转成一维数组:', this.imgArr.join().split(','));
			},
			// 获取上传图片basic
			getUpImgInfoBasic() {
				console.log('转成多维数组:', this.basicArr);
				console.log('转成一维数组:', this.basicArr.join().split(','));
			},
			// 获取上传图片阿里云
			getUpImgInfoOos() {
				console.log('转成多维数组:', this.oosArr);
				console.log('转成一维数组:', this.oosArr.join().split(','));
			},
			// 获取上传图片七牛云
			getUpImgInfoQiniu() {
				console.log('转成多维数组:', this.qiniuArr);
				console.log('转成一维数组:', this.qiniuArr.join().split(','));
			}
		}
	}
</script>


<style>

</style>
