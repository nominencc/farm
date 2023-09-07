<template>
  <div>
    
  <div class="echart1" v-if="isTudi2">
    <div id="echartsTitleToLeft" >土壤酸碱度&电导率2</div>
    <farm-chart-1 @begin2="addSnow2" @stop2="removeSnow2"></farm-chart-1>
  </div>
  <div class="echart4" v-if="isTudi1">
    <div id="echartsTitleToRight">土壤酸碱度&电导率1</div>
    <farm-chart-4 @begin1="addSnow1" @stop1="removeSnow1"></farm-chart-4>
  </div>
  <div class="echart7" v-if="isTudi3">
    <div id="echartsTitleToRight">土壤酸碱度&电导率3</div>
    <farm-chart-7 @begin3="addSnow3" @stop3="removeSnow3"></farm-chart-7>
  </div>
  <div class="echart2" v-if="isTudi2">
    <div id="echartsTitleToLeft">土壤温湿度2</div>
    <farm-chart-2 @begin2="addSnow2" @stop2="removeSnow2"></farm-chart-2>
  </div>
  <div class="echart5" v-if="isTudi1">
    <div id="echartsTitleToLeft">土壤温湿度1</div>
    <farm-chart-5 @begin1="addSnow1" @stop1="removeSnow1"></farm-chart-5>
  </div>
  <div class="echart8" v-if="isTudi3">
    <div id="echartsTitleToLeft">土壤温湿度3</div>
    <farm-chart-8 @begin3="addSnow3" @stop3="removeSnow3"></farm-chart-8>
  </div>

  <div class="echart3" v-if="isTudi2">
    <div id="echartsTitleToLeft">氮磷钾含量2</div>
    <farm-chart-3 @begin2="addSnow2" @stop2="removeSnow2"></farm-chart-3>
  </div>
  <div class="echart6" v-if="isTudi1">
    <div id="echartsTitleToLeft">氮磷钾含量1</div>
    <farm-chart-6 @begin1="addSnow1" @stop1="removeSnow1"></farm-chart-6>
  </div>
  <div class="echart9" v-if="isTudi3">
    <div id="echartsTitleToLeft">氮磷钾含量3</div>
    <farm-chart-9 @begin3="addSnow3" @stop3="removeSnow3"></farm-chart-9>
  </div>
  <div class="chart-wrapper" >
          <radder-chart></radder-chart>
  </div>
  <div class="table1">
    <table-one :Currentdata1="data1" :Currentdata2="data2" :Currentdata3="data3" ></table-one>
  </div>
  <div id="container" ></div>
  </div>
</template>

<script async src="https://unpkg.com/es-module-shims@1.6.3/dist/es-module-shims.js"></script>
<script>
import * as THREE from 'three'
import {OrbitControls} from 'three/examples/jsm/controls/OrbitControls.js'
import FarmChart1 from '@/components/Charts/FarmChart1.vue';
import FarmChart2 from '@/components/Charts/FarmChart2.vue';
import { loadObj } from '@/utils';
import * as BufferGeometryUtils from 'three/examples/jsm/utils/BufferGeometryUtils.js'  //BufferGeometry是面片、线或点几何体的有效表述。包括顶点位置，面片索引、法相量、颜色值、UV 坐标和自定义缓存属性值。使用 BufferGeometry 可以有效减少向 GPU 传输上述数据所需的开销。
import FarmChart3 from '@/components/Charts/FarmChart3.vue';
import FarmChart4 from '@/components/Charts/FarmChart4.vue';
import FarmChart5 from '@/components/Charts/FarmChart5.vue';
import FarmChart6 from '@/components/Charts/FarmChart6.vue';
import FarmChart7 from '@/components/Charts/FarmChart7.vue';
import FarmChart8 from '@/components/Charts/FarmChart8.vue';
import FarmChart9 from '@/components/Charts/FarmChart9.vue';
import RadderChart from '@/components/Charts/RadderChart.vue';
import TableOne from '@/components/Charts/TableOne.vue'
import {Sky} from 'three/examples/jsm/objects/Sky.js'
import axios from 'axios'
import md5 from "js-md5"
export default {
  components: { FarmChart1,FarmChart2,FarmChart3,RadderChart,FarmChart4,FarmChart5,FarmChart6,FarmChart7,FarmChart8,FarmChart9,TableOne},
  name: "WisdomFarm",
  data() {
    return {
      isTudi2: true,
      isTudi1: false,
      isTudi3: false,
      effectController : {
					turbidity: 10,
					rayleigh: 3,
					mieCoefficient: 0.005,
					mieDirectionalG: 0.7,
					elevation: 4,
					azimuth: 90,
					exposure: 0.5
				},
      config:{
          // range1:[270+170],
          count:500
        },
      pointList1: [],
      pointList2: [],
      pointList3: [],
      data1:{
        temperature:(Math.random() * 40.0).toFixed(1),
        humidity:(Math.random() * 65 + 5).toFixed(0),
        PHValue:(Math.random() * 5 + 4).toFixed(1),
        N:(Math.random() * 300 + 50).toFixed(0),
        P:(Math.random() * 480 + 20).toFixed(0),
        K:(Math.random() * 800 + 200).toFixed(0),
        conduct:(Math.random() * 65 + 5).toFixed(0),
      },
      data2:{
        temperature:(Math.random() * 40.0).toFixed(1),
        humidity:(Math.random() * 65 + 5).toFixed(0),
        PHValue:(Math.random() * 5 + 4).toFixed(1),
        N:(Math.random() * 300 + 50).toFixed(0),
        P:(Math.random() * 480 + 20).toFixed(0),
        K:(Math.random() * 800 + 200).toFixed(0),
        conduct:(Math.random() * 65 + 5).toFixed(0),
      },
      data3:{
        temperature:(Math.random() * 40.0).toFixed(1),
        humidity:(Math.random() * 65 + 5).toFixed(0),
        PHValue:(Math.random() * 5 + 4).toFixed(1),
        N:(Math.random() * 300 + 50).toFixed(0),
        P:(Math.random() * 480 + 20).toFixed(0),
        K:(Math.random() * 800 + 200).toFixed(0),
        conduct:(Math.random() * 65 + 5).toFixed(0),
      },
      headers:{
        Accesstoken:"e500f9884cd4ce06d0ade3ac163eccf4", 
        Appid:"1060859541382086656f0001",
        Keyid:"K.1060859541424029698",
        Nonce:Math.random().toString(), //随机字符串，每次请求都不同
        Time:new Date().getTime(),  //请求时间戳，单位毫秒
        Sign:"",  //请求签名
        Lang:"zh", //非必填
        AppKey:"3q3z9ax7czqji7fe8m69rqrbzmi4yr94"
        // ACCOUNT_ID:  1101220230418000000
        // ACCOUNT_NAME:  绿米测试账户5楼会议室
        // APP_ID:  1060859541382086656f0001
        // KEY_ID:  K.1060859541424029698
        // APP_KEY: 3q3z9ax7czqji7fe8m69rqrbzmi4yr94
        // ACCESS_TOKEN:  leddedd678a18559768bf5a1662e9a45
        // REFRESH_TOKEN: 08edfbc5cd1a9e556577608a1ale332d
        // REFRESH_TIME:  2023-08-2509:27:59
        // FLAG:  00
        // URL: http://10.224.52.152:2048/api
        // ACCOUNT_TYPE:  00
        // USER_CODE:  18148413696
        // AIR_CODE:  939144294com
      }
    }
  },
  mounted() {
    this.init()
    this.onWindowResize()
    this.addClickEvent()
    this.animateSnow1()
    this.animateSnow2()
    this.animateSnow3()
  },
  methods: {
    init() {
      this.scene = new THREE.Scene()
      this.snow1 = this.Snow1(this.scene)
      this.snow2 = this.Snow2(this.scene)
      // this.snow2 = new Snow2(this.scene)
      this.snow3 = this.Snow3(this.scene)
      // console.log("this.scene:",this.scene)
      loadObj('model/farm.glb')
       .then(gltf => {
          // console.log('gltf', gltf)
          //model 就是group
          let group = gltf.scene || gltf.scene[0]

          let geoArr = []

          group.updateMatrixWorld()
          group.traverse(child => {
            if (child.isMesh) {
              child.material.emissive = child.material.color
              child.material.emissiveMap = child.material.map
              geoArr.push(child.geometry.clone().applyMatrix4(child.matrixWorld))
            }
          })
          //获取3个土地对象
          this.tudis = []
          this.tudis.push(group.getObjectByName('tudi_1'))
          this.tudis.push(group.getObjectByName('tudi_2'))
          this.tudis.push(group.getObjectByName('tudi_3'))

          this.tudiBox1 = new THREE.Box3Helper(new THREE.Box3().setFromObject (this.tudis[0]).translate(new THREE.Vector3(0,0,-105)),0xffff00)
          this.tudiBox2 = new THREE.Box3Helper(new THREE.Box3().setFromObject (this.tudis[1]).translate(new THREE.Vector3(0,0,-95)),0xffff00)
          this.tudiBox3 = new THREE.Box3Helper(new THREE.Box3().setFromObject (this.tudis[2]).translate(new THREE.Vector3(0,0,-95)),0xffff00)

          this.scene.add(this.tudiBox2)

          let geo = BufferGeometryUtils.mergeGeometries(geoArr, false)
          let edge = new THREE.EdgesGeometry(geo)
          new THREE.LineSegments(edge, new THREE.LineBasicMaterial({
            color: 0xffffff
          }))
          group.position.set(0, 0, -100)
          this.scene.add(group)
        })

      this.time = {
        value: 0
      }
      this.clock = new THREE.Clock()
      const canvas = document.getElementById('container')
      const width = canvas.clientWidth
      const height = canvas.clientHeight
      // 添加平行光
      const directionLight = new THREE.DirectionalLight(new THREE.Color('#ffffff'))
      directionLight.position.set(-2000,700,0)
      this.scene.add(directionLight)
      // 添加环境光
      this.scene.add(new THREE.AmbientLight(new THREE.Color('#ffffff'), 1))
      // 创建相机
      this.camera = new THREE.PerspectiveCamera(60, width / height, 1, 2000000)
      //相机位置
      this.camera.position.set(-2500,1100,0)
      // 场景添加相机
      this.scene.add(this.camera)
       // 创建渲染器
      this.renderer = new THREE.WebGLRenderer()
      // 设置大小
      this.renderer.setSize(width, height)
      // this.renderer.setSize(canvas.clientWidth, canvas.clientHeight)
      // this.renderer.setSize( window.innerWidth, window.innerHeight )
      // 设置像素比
      this.renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2))
      //背景颜色
      this.renderer.setClearColor(new THREE.Color('#344b58'))

      this.renderer.toneMapping = THREE.ACESFilmicToneMapping
      this.renderer.toneMappingExposure = 0.5

      canvas.appendChild(this.renderer.domElement)

      // 创建控制器 OrbitControls轨道控制器
      this.controls = new OrbitControls(this.camera, this.renderer.domElement)
      //禁止缩放
      // this.controls.enableZoom = false
      //禁止右键拖动
      // this.controls.enablePan = false
      this.initSky()
			window.addEventListener( 'resize', this.onWindowResize );
      // 添加动画
      this.animate()
  
    },
    initSky(){
      this.sky = new Sky()
      this.sky.scale.setScalar( 450000 ) 
      
      this.sun = new THREE.Vector3()
      this.scene.add(this.sky)

      const uniforms = this.sky.material.uniforms;
      uniforms[ 'turbidity' ].value = this.effectController.turbidity;
			uniforms[ 'rayleigh' ].value = this.effectController.rayleigh;
			uniforms[ 'mieCoefficient' ].value = this.effectController.mieCoefficient;
			uniforms[ 'mieDirectionalG' ].value = this.effectController.mieDirectionalG;
 
      const phi = THREE.MathUtils.degToRad( this.effectController.azimuth - this.effectController.elevation);
      const theta = THREE.MathUtils.degToRad(this.effectController.azimuth);
      
      //Spherical( radius : 半径, phi : 与y轴的极角, theta : 绕y轴的赤道角 )
      this.sun.setFromSphericalCoords( 1, phi, theta );
      
      uniforms[ 'sunPosition' ].value.copy( this.sun );

      this.renderer.toneMappingExposure = this.effectController.exposure
      this.renderer.render(this.scene,this.camera)
    },
    onWindowResize() {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();
      // this.renderer.setSize( window.innerWidth, window.innerHeight );
      this.render();
      // console.log("88888888:",this.effectController.azimuth)
    },
    render(){
      this.renderer.render(this.scene,this.camera)
    },
    addClickEvent() {
      window.addEventListener('click', event => {
        // 获取XY
        let x = (event.clientX / window.innerWidth) * 2 - 1
        let y = - (event.clientY / window.innerHeight) * 2 + 1
        //显示x,y坐标
        // console.log('x=' + x, 'y=' + y)
        // 标准向量
        let vec = new THREE.Vector3(x, y, -1)
        // 转化为世界坐标
        vec.unproject(this.camera)
        // THREE.Vector2 = new THREE.Vector2(x,y)
        // 归一化
        let ray = vec.sub(this.camera.position).normalize()
        //光线投射，用于进行鼠标拾取
        let raycaster = new THREE.Raycaster(this.camera.position, ray)

        let objs = raycaster.intersectObjects(this.tudis, true)
        //显示点击的土地名称
        if (objs.length) {
          //打印点击的对象名称
          // console.log(objs)
          console.log(objs[0].object.name)
          
        if (objs[0].object.name === 'tudi_1') {
          this.isTudi2 = false
          this.scene.remove(this.tudiBox2)
          this.isTudi3 = false
          this.scene.remove(this.tudiBox3)
          if(this.isTudi1 === false){
              this.isTudi1 = true
              this.scene.add(this.tudiBox1)
          }
          else{
            this.isTudi1 = false
            // console.log('this.scene', this.scene)
            this.scene.remove(this.tudiBox1) 
            // console.log('this.aaas', this.scene)
          }
        }
        if (objs[0].object.name === 'tudi_2') {
          this.isTudi1 = false
          this.scene.remove(this.tudiBox1)
          this.isTudi3 = false
          this.scene.remove(this.tudiBox3)
          if(this.isTudi2 === false){
              this.isTudi2 = true
              this.scene.add(this.tudiBox2)
          }
          else{
            this.isTudi2 = false
            this.scene.remove(this.tudiBox2)
          }
        }
        if (objs[0].object.name === 'tudi_3') {
          this.isTudi1 = false
          this.scene.remove(this.tudiBox1)
          this.isTudi2 = false
          this.scene.remove(this.tudiBox2)
          if(this.isTudi3 === false){
              this.isTudi3 = true
              this.scene.add(this.tudiBox3)
          }
          else{
            this.isTudi3 = false
            this.scene.remove(this.tudiBox3)
          }
        }
        }
      })
    },
    Snow1(){
          const geometry = new THREE.BufferGeometry()
          this.pointList1 = []
          for (let i = 0; i < this.config.count; i++) {
            let vec = new THREE.Vector3(
              Math.random() * (600+500) - 600,
              Math.random() * 250,
              Math.random() * (270+170) - 770
            )
            // vec.speedX = Math.random() * 0.8 - 0.4
            vec.speedY =  1 + 0.1
            // vec.speedZ = Math.random() * 0.8 - 0.4

            this.pointList1.push(vec)
          }
          geometry.setFromPoints(this.pointList1)

          //PointsMaterial点材质
          const material = new THREE.PointsMaterial({
            transparent: true,
            map: new THREE.TextureLoader().load(require('../../assets/water.png')),
            size: 20,
            depthTest: true,
            // color:"#ffffff"
          })

          return this.points1 = new THREE.Points(geometry,material)
    },
    animateSnow1(){   
    this.pointList1.forEach(vec => {

      vec.y -=vec.speedY

      if(vec.y < 15) {
        vec.x = Math.random() * (600+500) - 600 ,
        // vec.x = Math.random() * this.config.range - this.config.range/2
        vec.y = Math.random() * 250
        // vec.z = Math.random() * this.config.range - this.config.range / 2
        vec.z = Math.random() * (270+170) - 770

        // vec.speedX = Math.random() * 0.8 - 0.4
        vec.speedY =  1 + 0.1
        // vec.speedZ = Math.random() * 0.8 - 0.4
      } else {
        // vec.x -=vec.speedX
        // vec.z -=vec.speedZ
      }
      })
      this.points1.geometry.setFromPoints(this.pointList1)

      requestAnimationFrame(this.animateSnow1.bind(this))
      // requestAnimationFrame(this.animate1.bind(this))

    },
    addSnow1(){
      this.scene.add(this.points1)
    },
    removeSnow1(){
      this.scene.remove(this.points1)
    },
    Snow2(){
          const geometry = new THREE.BufferGeometry()
          this.pointList2 = []
          for (let i = 0; i < this.config.count; i++) {
            let vec = new THREE.Vector3(
              Math.random() * (600+500) - 600,
              Math.random() * 250,
              Math.random() * (270+170) - 170
            )
            // vec.speedX = Math.random() * 0.8 - 0.4
            vec.speedY =  1 + 0.1
            // vec.speedZ = Math.random() * 0.8 - 0.4

            this.pointList2.push(vec)
          }
          geometry.setFromPoints(this.pointList2)

          const material = new THREE.PointsMaterial({
            transparent: true,
            map: new THREE.TextureLoader().load(require('../../assets/water.png')),
            size: 20,
            depthTest: true,
            // color:"#ffffff"
          })

          return this.points2 = new THREE.Points(geometry,material)
    },
    animateSnow2(){   
    this.pointList2.forEach(vec => {

      vec.y -=vec.speedY

      if(vec.y < 15) {
        vec.x = Math.random() * (600+500) - 600 ,
        // vec.x = Math.random() * this.config.range - this.config.range/2
        vec.y = Math.random() * 250
        // vec.z = Math.random() * this.config.range - this.config.range / 2
        vec.z = Math.random() * (270+170) - 170

        // vec.speedX = Math.random() * 0.8 - 0.4
        vec.speedY =  1 + 0.1
        // vec.speedZ = Math.random() * 0.8 - 0.4
      } else {
        // vec.x -=vec.speedX
        // vec.z -=vec.speedZ
      }
      })
      this.points2.geometry.setFromPoints(this.pointList2)

      requestAnimationFrame(this.animateSnow2.bind(this))
      // requestAnimationFrame(this.animate1.bind(this))

    },
    addSnow2(){
      this.scene.add(this.points2)

      axios({
        //请求方式post或get 默认get
        method:"post",
        //请求的地址
        url:"/api",    
        //请求头参数
        headers:{
          Accesstoken:this.headers.Accesstoken,
          Appid:this.headers.Appid,
          Keyid:this.headers.Keyid,
          Nonce:this.headers.Nonce,
          Time:this.headers.Time,
          Sign:this.createSign(),
          Lang:this.headers.Lang,
        },
        
        //请求参数
        data:{
          //请求意图 
          intent:"write.resource.device",       
          //请求数据 
          data:[
            {
            subjectId:"lumi.4cf8cdf3c83f979",
            resources:[
              {
                resourceId:"4.1.85",
                value:1
              }
            ]
         }
        ]
      },
        timeout:10000,
        
      },
      )
      .then((res) =>{
        //请求成功后想做的事
        console.log('success',res)
      })
      .catch(function(err){
        //请求失败后想做的事
        console.log('failed',err)
      })
    },
    removeSnow2(){
      this.scene.remove(this.points2)

      axios({
        //请求方式post或get 默认get
        method:"post",
        //请求的地址
        url:"/api",    
        //请求头参数
        headers:{
          Accesstoken:this.headers.Accesstoken,
          Appid:this.headers.Appid,
          Keyid:this.headers.Keyid,
          Nonce:this.headers.Nonce,
          Time:this.headers.Time,
          Sign:this.createSign(),
          Lang:this.headers.Lang,
        },
        
        //请求参数
        data:{
          //请求意图 
          intent:"write.resource.device",       
          //请求数据 
          data:[
            {
            subjectId:"lumi.4cf8cdf3c83f979",
            resources:[
              {
                resourceId:"4.1.85",
                value:0
              }
            ]
         }
        ]
      },
        timeout:20000,
        
      },
      )
      .then((res) =>{
        //请求成功后想做的事
        console.log('success',res)
      })
      .catch(function(err){
        //请求失败后想做的事
        console.log('failed',err)
      })
      
    },
    createSign(){
            let sign=this.headers.Sign
            //Accesstoken、Appid、Keyid、Nonce、Time拼接
            sign=sign+"Accesstoken="+this.headers.Accesstoken+"&"+"Appid="+this.headers.Appid+"&"+"Keyid="+this.headers.Keyid+"&"+"Nonce="+this.headers.Nonce+"&"+"Time="+this.headers.Time
            //末尾拼接AppKey
            sign=sign+this.headers.AppKey
            //字符串小写
            sign=sign.toString().toLowerCase()
            //MD5 32位加密
            sign=md5(sign)
            console.log("sign:",sign)
            return sign
          },
    Snow3(){
          const geometry = new THREE.BufferGeometry()
          this.pointList3 = []
          for (let i = 0; i < this.config.count; i++) {
            let vec = new THREE.Vector3(
              Math.random() * (600+500) - 600,
              Math.random() * 250,
              Math.random() * (270+170) + 440
            )
            // vec.speedX = Math.random() * 0.8 - 0.4
            vec.speedY =  1 + 0.1
            // vec.speedZ = Math.random() * 0.8 - 0.4

            this.pointList3.push(vec)
          }
          geometry.setFromPoints(this.pointList3)

          const material = new THREE.PointsMaterial({
            transparent: true,
            map: new THREE.TextureLoader().load(require('../../assets/water.png')),
            size: 20,
            depthTest: true,
            // color:"#ffffff"
          })

          return this.points3 = new THREE.Points(geometry,material)
    },
    animateSnow3(){   
    this.pointList3.forEach(vec => {

      vec.y -=vec.speedY

      if(vec.y < 15) {
        vec.x = Math.random() * (600+500) - 600 ,
        // vec.x = Math.random() * this.config.range - this.config.range/2
        vec.y = Math.random() * 250
        // vec.z = Math.random() * this.config.range - this.config.range / 2
        vec.z = Math.random() * (270+170) + 440

        // vec.speedX = Math.random() * 0.8 - 0.4
        vec.speedY =  1 + 0.1
        // vec.speedZ = Math.random() * 0.8 - 0.4
      } else {
        // vec.x -=vec.speedX
        // vec.z -=vec.speedZ
      }
      })
      this.points3.geometry.setFromPoints(this.pointList3)

      requestAnimationFrame(this.animateSnow3.bind(this))
      // requestAnimationFrame(this.animate1.bind(this))

    },
    addSnow3(){
      this.scene.add(this.points3)
    },
    removeSnow3(){
      this.scene.remove(this.points3)
    },
    // start(){
    // this.time.value += this.clock.getDelta()
    // },
    animate() {
    // this.start()
    // this.azimuth = 100
    // console.log(this.azimuth)
    this.controls.update()
    this.renderer.render(this.scene, this.camera)
    requestAnimationFrame(this.animate)
    },
  }   
}

</script>

<style scoped>

.table1{
  position: absolute;
  left:0%;
  top:20px
}
table{
    /* 阴影 */
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
    /*取消表格边框 */
    border-collapse:collapse;
    letter-spacing:1px;/* 文字缩进 */
}
table,th,tr,td{ 
    /* 表格横线 */
    border-bottom:1px solid #ffffff;
    padding:7px;
    color:#fcf7f7cc
}
#n1 {
  text-align: left;
}

.echart2,.echart5,.echart8{
  position: fixed;
  right:0%;
  top:1%;
}
.echart1,.echart4,.echart7{
  position: fixed;
  right:0px;
  top:67%;
}
.echart3,.echart6,.echart9{
  position:fixed;
  right:0px;
  top: 34%;
  /* opacity:0.5 */

}
.chart-wrapper {
    position:fixed;
    top:29%;
    left:-3%;
  }
#echartsTitleToLeft{
  /* background-color: #08609366; */
  background: transparent;
  /* background: linear-gradient(to left,#08609399,#344b5800); */
  text-align:center;
  color:#fff;
  font-size: 15;
}
#echartsTitleToRight{
  /* background-color: #08609366; */
  background: transparent;
  /* background: linear-gradient(to right,#08609399,#344b5800); */
  text-align:center;
  color:#fff;
  font-size: 15;
}
#container {
  height: 100vh;
  width: 100vw;
}
</style>
