<template>
	<view>
		<view id="center">

		</view>
	</view>
</template>
<script>
	export default {

		mounted() {

		},
		methods: {
			onViewClick(options) {
				console.log('uni接收点击事件');
				console.log(options.data);
			},
		}
	}
</script>

<script module="three" lang="renderjs">
	import {
		OrbitControls
	} from '@/static/js/OrbitControls.js'
	const THREE = require('@/static/js/three.js')

	var that;
	let myThree;
	var renderer;
	var scene;
	var camera;
	var mesh;
	var controls;
	var materials;
	export default {
		data() {
			return {}
		},
		created() {

		},

		mounted() {
			that = this;


			this.initThree(); //加载场景
			this.createControls(); //控制模型的缩放、平移、旋转操作
			setTimeout(() => {
				this.animate()
			}, 200)
		},
		methods: {
			createControls() {
				controls = new OrbitControls(camera, renderer.domElement)
			},
			initThree() {
				// 如果返回的不是未定义，说明threejs成功引入
				console.log('打印场景API', THREE.Scene);
				/* 创建场景对象Scene */
				scene = new THREE.Scene();
				/* 
				    创建网格模型
				 */
				var geometry = new THREE.BoxGeometry(20, 20, 20);; // 创建一个立方几何对象（长宽高）

				/*
				    光源设置
				 */

				//  点光源--光照强度
				var point = new THREE.PointLight(0xffff00);
				point.position.set(10, 0, 10); // 点光源位置
				scene.add(point); // 点光源添加到场景中
				// 环境光
				var ambient = new THREE.AmbientLight(0x444444);
				scene.add(ambient);
				/* 
				    相机设置
				 */
				var width = 0.9 * window.innerWidth; // 窗口宽度
				var height = 300; // 高度
				var k = width / height; // 窗口宽高比
				var s = 12; // 三维场景显示范围控制系数，系数越大，显示的范围越大
				// 创建相机对象（正射投影）
				camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000);
				camera.position.set(100, 0, 200); // 设置相机位置
				camera.lookAt(scene.position); // 设置相机的方向（指向场景对象）
				/* 
				    创建渲染器对象
				 */
				renderer = new THREE.WebGLRenderer({
					antialias: true //是否执行抗锯齿
				});
				renderer.setSize(width, height); // 设置渲染区域尺寸
				// renderer.setClearColor(0xb9d3ff, 1); // 设置背景颜色
				document.getElementById('center').appendChild(renderer.domElement); // body元素中插入canvas对象
				// 执行渲染操作，指定场景，相机作为参数
				renderer.render(scene, camera);

				// 换成自己的图片路径
				var loader = new THREE.TextureLoader();

				let one = loader.load('static/image/6.png'); //左右
				let two = loader.load('static/image/1.png'); //左右

				let three = loader.load('static/image/5.png'); //上
				let four = loader.load('static/image/5.png'); //下

				let five = loader.load('static/image/3.png');
				let six = loader.load('static/image/2.png');


				const material1 = new THREE.MeshBasicMaterial({
					map: one
				});
				const material2 = new THREE.MeshBasicMaterial({
					map: two
				});
				const material3 = new THREE.MeshBasicMaterial({
					map: three
				});
				const material4 = new THREE.MeshBasicMaterial({
					map: four
				});
				const material5 = new THREE.MeshBasicMaterial({
					map: five
				});
				const material6 = new THREE.MeshBasicMaterial({
					map: six
				});


				materials = [material1, material2, material3, material4, material5, material6]
				mesh = new THREE.Mesh(geometry, materials); // 网格模型对象
				scene.add(mesh); // 网格模型添加到场景中

				mesh.geometry.scale(1, 1, -1)

				//为了防止这种情况的发生，我们只需要将摄像机稍微向外移动一些即可。
				camera.position.set = (0, 0, 0.01);
				camera.position.z = 1;

			},
			// 动画
			animate() {
				requestAnimationFrame(this.animate);
				// 让它旋转起来
				// mesh.rotation.y += 0.01;
				renderer.render(scene, camera);
			},
		},

	}
</script>

<style lang="less" scoped>
.center{
	width: 100vw;
	height: 100vh;
}
</style>
