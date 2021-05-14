---
title: hexo使用大全
date: 2021-05-12 20:42:55
tags: [hexo,博客相关]
categories: 博客相关
---

## 一、hexo的butterfly主题支持moc3格式的l2d

* 本教程参考了 guoshidu( https://github.com/guoshidu ) 的实现，不过他是在yilia主题的基础上实现的，详见issue: https://github.com/guoshidu/hexo-live2d-moc3/issues/1
* 本教程需要魔改butterfly，本教程基于hexo-theme-butterfly 3.7.6版本，安装方式是通过github安装到themes文件夹下面。而不是通过npm install进行的安装，其注意区别
* 本教程中将butterfly文件夹中的_config.yml的内容拷贝到了主项目根目录的_`_config.butterfly.yml`文件中，hexo会将主目录的`_config.butterfly.yml、_config.yml和butterfly文件夹中的_config.yml`自动进行合并，重复的键优先级主目录>butterfly文件夹，`_config.butterfly.yml`优先级高于`_config.yml`

### （一）引入js

* 修改`_config.butterfly.yml的`配置如下,在head标签中插入要使用的l2d相关js文件

```yaml
# Inject
# Insert the code to head (before '</head>' tag) and the bottom (before '</body>' tag)
# 插入代码到头部 </head> 之前 和 底部 </body> 之前
inject:
  head:
    # - <link rel="stylesheet" href="/xxx.css">
    - <script src="https://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script> 
    - <script src="https://cdn.jsdelivr.net/gh/litstronger/live2d-moc3@master/js/frame/live2dcubismcore.min.js"></script>
    - <script src="https://cdnjs.cloudflare.com/ajax/libs/pixi.js/4.6.1/pixi.min.js"></script>
    - <script src="https://cdn.jsdelivr.net/gh/litstronger/live2d-moc3@master/js/live2dcubismframework.js"></script>
    - <script src="https://cdn.jsdelivr.net/gh/litstronger/live2d-moc3@master/js/live2dcubismpixi.js"></script>
    - <script src="https://cdn.jsdelivr.net/gh/litstronger/live2d-moc3@master/js/l2d.js"></script>
  bottom:
    - <div class="aplayer no-destroy" data-id="3083316116" data-server="netease" data-type="playlist" data-fixed="true" data-mini="true" data-listFolded="false" data-order="random" data-preload="none" data-autoplay="true" muted></div>
    # - <script src="xxxx"></script>
```

### （二）魔改butterfly
* 到github上将hexo-theme-butterfly fork到自己的仓库
* 将项目中引入的butterfly修改为自己fork的仓库地址，这是为了魔改以后能够push代码，注意.gitmodules文件中子仓库地址的修改
* 修改themes/butterfly/source/main.js内容，在初始化Header之后加入下面一段代码
```javascript
//添加moc3 l2d 开始
  //1、添加l2d容器
  let l2dContainerNode = document.createElement("div")
  l2dContainerNode.innerHTML = `
    <div id="audiodiv" style="display:none">
      <audio id="qgirlvoiceAudio" src="" controls="controls">
      </audio>
    </div>
    <div id="qgirlMessage" style="visibility:hidden;position: fixed; opacity: 1; right: 30px; bottom: 190px;z-index:99;width:160px;background-color:#6969698a;color:white;padding:10px;font-size:12px;border-radius:20px">店长，不查看一下终端的信息吗？……或许，有很重要的事呢</div>  

    <div class="Canvas" id="L2dCanvas" style="position: fixed; opacity: 1; right: 30px; bottom: -160px;z-index:99">
      <canvas width="400" height="640" style="touch-action: none; cursor: inherit; width: 400px; height: 640px;"></canvas>
    </div>
  `
  document.body.append(l2dContainerNode)
  //2、添加script标签
  let script = document.createElement("script");
  script.innerHTML = `
  class Viewer {
    constructor(config) {
      let width = config.width || 800;
      let height = config.height || 600;
      let role = config.role;
      let left = config.left; //|| '0px'
      let top = config.top; //|| '0px'
      let right = config.right; //|| '0px'
      let bottom = config.bottom; //|| '0px'
      let bg = config.background;
      let opa = config.opacity;
      let mobile = config.mobile;
  
      if (!mobile) {
        if (this.isMobile()) return;
      }
      this.l2d = new L2D(config.basePath);
      this.canvas = $(".Canvas");
  
      this.l2d.load(role, this);
      this.app = new PIXI.Application({
        width: width,
        height: height,
        transparent: true,
        antialias: true, // 抗锯齿
      });
      this.canvas.html(this.app.view);
      this.canvas[0].style.position = "fixed";
      if (bg) {
        this.canvas[0].style.background = "url('"+ bg + "')";
        this.canvas[0].style.backgroundSize = "cover";
      }
      if (opa) this.canvas[0].style.opacity = opa;
      if (top) this.canvas[0].style.top = top;
      if (right) this.canvas[0].style.right = right;
      if (bottom) this.canvas[0].style.bottom = bottom;
      if (left) this.canvas[0].style.left = left;
  
      this.app.ticker.add((deltaTime) => {
        if (!this.model) {
          return;
        }
  
        this.model.update(deltaTime);
        this.model.masks.update(this.app.renderer);
      });
  
      window.onresize = (event) => {
        if (event === void 0) {
          event = null;
        }
  
        this.app.view.style.width = width + "px";
        this.app.view.style.height = height + "px";
        this.app.renderer.resize(width, height);
  
        if (this.model) {
          this.model.position = new PIXI.Point(width * 0.45, height * 0.36);
          // this.model.scale = new PIXI.Point((this.model.position.x * 0.6), (this.model.position.x * 0.6));
          //修改moc3模型显示的放大倍数，这里用的是双生视界的moc3模型，它有点特殊，模型普遍偏小，所以这里面倍数变大，如果你的模型显示太大，请修改这里的放大倍数
          this.model.scale = new PIXI.Point(width * 1, height * 0.6);
          this.model.masks.resize(this.app.view.width, this.app.view.height);
        }
      };
      this.isClick = false;
      this.app.view.addEventListener("mousedown", (event) => {
        this.isClick = true;
      });
      this.app.view.addEventListener("mousemove", (event) => {
        if (this.isClick) {
          this.isClick = false;
          if (this.model) {
            this.model.inDrag = true;
          }
        }
  
        if (this.model) {
          let mouse_x = this.model.position.x - event.offsetX;
          let mouse_y = this.model.position.y - event.offsetY;
          this.model.pointerX = -mouse_x / this.app.view.height;
          this.model.pointerY = -mouse_y / this.app.view.width;
        }
      });

      //语音数组，播放语音的地址
      const arrVoice = ["https://www.joy127.com/url/77696.mp3","https://www.joy127.com/url/77697.mp3","https://www.joy127.com/url/77698.mp3","https://www.joy127.com/url/77699.mp3","https://www.joy127.com/url/77700.mp3","https://www.joy127.com/url/77701.mp3"]
      //语音对应的文字
      const arrText = ["店长要来一杯咖啡么?","愿所有的牺牲者得以安息","一定不存在不需要战争也能解决纠纷的办法","店长，不查看一下终端的信息吗？……或许，有很重要的事呢","店长今天的检查结果也ok","咖啡馆里又发生了什么呢？要不要去看看"]
      this.app.view.addEventListener("mouseup", (event) => {
        if (!this.model) {
          return;
        }
        this.isClick = true;
        if (this.isClick) {
          console.log("onclicked")
          if (this.isHit("TouchHead", event.offsetX, event.offsetY)) {
            this.startAnimation("touch_head", "base");
          } else if (this.isHit("TouchSpecial", event.offsetX, event.offsetY)) {
            this.startAnimation("touch_special", "base");
          } else {
            //你所使用的moc3模型的motions文件夹下的所有motions名称数组
            const bodyMotions = ["Mgirl07_baohaibao","Mgirl07_baohaibao_a","Mgirl07_baolu_c","Mgirl07_dahaqian_c","Mgirl07_dazhaohu_a","Mgirl07_diantou","Mgirl07_ganga_c","Mgirl07_haixiu","Mgirl07_jiashengqi","Mgirl07_jingxi_a","Mgirl07_jingya","Mgirl07_keai_a","Mgirl07_kunao","Mgirl07_motouweixiao","Mgirl07_motouwushi_c","Mgirl07_motouxiao_a","Mgirl07_nu","Mgirl07_qidao","Mgirl07_qinwenshizijia_a","Mgirl07_sajiao_a","Mgirl07_sikao_a","Mgirl07_stand","Mgirl07_stand_a","Mgirl07_stand_c","Mgirl07_tianxiao_a","Mgirl07_touteng_c","Mgirl07_tuoyeganga_c","Mgirl07_tuoyehaixiu","Mgirl07_tuoyehaixiu_a","Mgirl07_tuoyeyihuo","Mgirl07_wuzhu","Mgirl07_xianqi_c","Mgirl07_xianqunzi","Mgirl07_xianqunzi_a","Mgirl07_xiugongpai_a","Mgirl07_xiunu_a","Mgirl07_yihuo"];
            //点击随机一个motion
            let currentMotion =
              bodyMotions[Math.floor(Math.random() * bodyMotions.length)];
            //随机播放语音1-6
            let rand = Math.floor(Math.random()*6);
            $("#qgirlvoiceAudio")[0].src = arrVoice[rand];
            $("#qgirlvoiceAudio")[0].play();
            //显示对应文字,2秒后将其关闭
            const node = document.getElementById("qgirlMessage")
            node.style.visibility = "visible"

            node.innerText = arrText[rand]
            setTimeout(()=>{
              node.style.visibility = "hidden"
            },2000)

            this.startAnimation(currentMotion, "base");
          }
        }
  
        this.isClick = false;
        this.model.inDrag = false;
      });
      console.log("Init finished.");
    }
  
    changeCanvas(model) {
      this.app.stage.removeChildren();
  
      model.motions.forEach((value, key) => {
        if (key != "effect") {
          let btn = document.createElement("button");
          let label = document.createTextNode(key);
          btn.appendChild(label);
          btn.className = "btnGenericText";
          btn.addEventListener("click", () => {
            this.startAnimation(key, "base");
          });
        }
      });
  
      this.model = model;
      this.model.update = this.onUpdate; // HACK: use hacked update fn for drag support
      // console.log(this.model);
      this.model.animator.addLayer(
        "base",
        LIVE2DCUBISMFRAMEWORK.BuiltinAnimationBlenders.OVERRIDE,
        1
      );
  
      this.app.stage.addChild(this.model);
      this.app.stage.addChild(this.model.masks);
  
      window.onresize();
    }
  
    onUpdate(delta) {
      let deltaTime = 0.016 * delta;
  
      if (!this.animator.isPlaying) {
        let m = this.motions.get("idle");
        this.animator.getLayer("base").play(m);
      }
      this._animator.updateAndEvaluate(deltaTime);
  
      if (this.inDrag) {
        this.addParameterValueById("ParamAngleX", this.pointerX * 30);
        this.addParameterValueById("ParamAngleY", -this.pointerY * 30);
        this.addParameterValueById("ParamBodyAngleX", this.pointerX * 10);
        this.addParameterValueById("ParamBodyAngleY", -this.pointerY * 10);
        this.addParameterValueById("ParamEyeBallX", this.pointerX);
        this.addParameterValueById("ParamEyeBallY", -this.pointerY);
      }
  
      if (this._physicsRig) {
        this._physicsRig.updateAndEvaluate(deltaTime);
      }
  
      this._coreModel.update();
  
      let sort = false;
      for (let m = 0; m < this._meshes.length; ++m) {
        this._meshes[m].alpha = this._coreModel.drawables.opacities[m];
        this._meshes[m].visible = Live2DCubismCore.Utils.hasIsVisibleBit(
          this._coreModel.drawables.dynamicFlags[m]
        );
        if (
          Live2DCubismCore.Utils.hasVertexPositionsDidChangeBit(
            this._coreModel.drawables.dynamicFlags[m]
          )
        ) {
          this._meshes[m].vertices = this._coreModel.drawables.vertexPositions[m];
          this._meshes[m].dirtyVertex = true;
        }
        if (
          Live2DCubismCore.Utils.hasRenderOrderDidChangeBit(
            this._coreModel.drawables.dynamicFlags[m]
          )
        ) {
          sort = true;
        }
      }
  
      if (sort) {
        this.children.sort((a, b) => {
          let aIndex = this._meshes.indexOf(a);
          let bIndex = this._meshes.indexOf(b);
          let aRenderOrder = this._coreModel.drawables.renderOrders[aIndex];
          let bRenderOrder = this._coreModel.drawables.renderOrders[bIndex];
  
          return aRenderOrder - bRenderOrder;
        });
      }
  
      this._coreModel.drawables.resetDynamicFlags();
    }
  
    startAnimation(motionId, layerId) {
      if (!this.model) {
        return;
      }
      console.log("Animation:", motionId, layerId);
      let m = this.model.motions.get(motionId);
      console.log("motionId:", m);
      if(m){m.loop = false};
      if (!m) {
        return;
      }
  
      let l = this.model.animator.getLayer(layerId);
      console.log("layerId:", l)
      if (!l) {
        return;
      }
  
      l.play(m);
    }
  
    isHit(id, posX, posY) {
      if (!this.model) {
        return false;
      }
  
      let m = this.model.getModelMeshById(id);
      if (!m) {
        return false;
      }
  
      const vertexOffset = 0;
      const vertexStep = 2;
      const vertices = m.vertices;
  
      let left = vertices[0];
      let right = vertices[0];
      let top = vertices[1];
      let bottom = vertices[1];
  
      for (let i = 1; i < 4; ++i) {
        let x = vertices[vertexOffset + i * vertexStep];
        let y = vertices[vertexOffset + i * vertexStep + 1];
  
        if (x < left) {
          left = x;
        }
        if (x > right) {
          right = x;
        }
        if (y < top) {
          top = y;
        }
        if (y > bottom) {
          bottom = y;
        }
      }
  
      let mouse_x = m.worldTransform.tx - posX;
      let mouse_y = m.worldTransform.ty - posY;
      let tx = -mouse_x / m.worldTransform.a;
      let ty = -mouse_y / m.worldTransform.d;
  
      return left <= tx && tx <= right && top <= ty && ty <= bottom;
    }
  
    isMobile() {
      var WIN = window;
      var LOC = WIN["location"];
      var NA = WIN.navigator;
      var UA = NA.userAgent.toLowerCase();
      function test(needle) {
        return needle.test(UA);
      }
      var IsAndroid = test(/android|htc/) || /linux/i.test(NA.platform + "");
      var IsIPhone = !IsAndroid && test(/ipod|iphone/);
      var IsWinPhone = test(/windows phone/);
      var device = {
        IsAndroid: IsAndroid,
        IsIPhone: IsIPhone,
        IsWinPhone: IsWinPhone,
      };
      var documentElement = WIN.document.documentElement;
      for (var i in device) {
        if (device[i]) {
          documentElement.className += " " + i.replace("Is", "").toLowerCase();
        }
      }
      return device.IsAndroid || device.IsIPhone || device.IsWinPhone;
    }
  }
  
  //let width = document.documentElement.clientWidth
  // let height = document.documentElement.clientHeight
  //moc3模型的在线地址，使用jsdelivr加速github仓库访问
  //可以自建gitHub仓库，将moc3 l2d提交到仓库（注意一个仓库最多只有50M）
  //因为jsdelivr对超过50M的仓库不予加速
  //jsdelivr的使用请自行百度
  var config = {
    width: 200,
    height: 320,
    right: "0px",
    bottom: "-100px",
    basePath: "https://cdn.jsdelivr.net/gh/Hexi1997/ailin@1.0/assets",
    role: "sifu",
    background: false,
    opacity: 1,
    mobile: false
  };
  var v = new Viewer(config);
  `;
```

### （三）效果如下（语音+文字+l2d)

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/image-20210513224142029.png" alt="image-20210513224142029" style="zoom:50%;" />



