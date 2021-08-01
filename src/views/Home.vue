<template>
  <div>
    <loading :progress="progress" :info="info"></loading>
    <keana-blog
      v-if="ready"
      :illustration="illustration"
      :mineral="mineral"
      :inkWash="inkWash"
      :photography="photography"
      :scene="other"
    ></keana-blog>
  </div>
</template>
<script>
import Loading from "../components/Loading.vue";
import KeanaBlog from "./KeanaBlog.vue";

const payload = {
  banner: [
    require("../assets/images/banner/b1.jpg"),
    require("../assets/images/banner/b2.jpg"),
    require("../assets/images/banner/b3.jpg"),
    require("../assets/images/banner/b4.jpg"),
    require("../assets/images/banner/b5.jpg"),
    require("../assets/images/banner/b6.jpg"),
    require("../assets/images/banner/b7.jpg"),
    require("../assets/images/banner/b8.jpg"),
    require("../assets/images/banner/b9.jpg"),
    require("../assets/images/banner/b10.jpg"),
    require("../assets/images/banner/jj.png"),
  ],
  illustration: [
    require("../assets/images/works/illustration.sunrise.4.3.jpg"),
    require("../assets/images/works/illustration.snow.3.4.jpg"),
    require("../assets/images/works/illustration.yue.4.3.jpg"),
    require("../assets/images/works/illustration.girl.9.16.jpg"),
    require("../assets/images/works/illustration.peach.9.16.jpg"),
    require("../assets/images/works/illustration.coffee.9.16.jpg"),
    require("../assets/images/works/illustration.coffee2.1.1.jpg"),
    require("../assets/images/works/illustration.three.16.9.jpg"),
    require("../assets/images/works/illustration.room.16.9.jpg"),
    require("../assets/images/works/illustration.forest.16.9.jpg"),
    require("../assets/images/works/illustration.wind.16.9.jpg"),
    require("../assets/images/works/illustration.kk.16.9.jpg"),
    require("../assets/images/works/illustration.three-1.9.16.jpg"),
    require("../assets/images/works/illustration.three-2.9.16.jpg"),
    require("../assets/images/works/illustration.three-3.9.16.jpg"),
    require("../assets/images/works/illustration.six.9.16.jpg"),
    require("../assets/images/works/illustration.ric.9.16.jpg"),
    require("../assets/images/works/illustration.f.3.4.jpg"),
    require("../assets/images/works/illustration.whale.1.1.jpg"),
    require("../assets/images/works/illustration.r.9.16.jpg"),
  ],
  mineral: [
    require("../assets/images/works/mineral.qing.16.9.jpg"),
    require("../assets/images/works/mineral.qin.16.9.jpg"),
    require("../assets/images/works/mineral.shuimu.16.9.jpg"),
    require("../assets/images/works/mineral.win.16.9.jpg"),
    require("../assets/images/works/mineral.fish.9.16.jpg"),
    require("../assets/images/works/mineral.fenghuo.9.16.jpg"),
    require("../assets/images/works/mineral.yan.16.9.jpg"),
    require("../assets/images/works/mineral.denglong.16.9.jpg"),
    require("../assets/images/works/mineral.zuma.16.9.jpg"),
    require("../assets/images/works/mineral.shanzi.3.4.jpg"),
    require("../assets/images/works/mineral.jiu.3.4.jpg"),
    require("../assets/images/works/mineral.kong.16.9.jpg"),
    require("../assets/images/works/mineral.linmo.3.4.jpg"),
    require("../assets/images/works/mineral.linmo2.3.4.jpg"),
    require("../assets/images/works/mineral.sakura4.3.jpg"),
    require("../assets/images/works/mineral.us.3.4.jpg"),
    require("../assets/images/works/mineral.use.3.4.jpg"),
    require("../assets/images/works/mineral.yinyue.9.16.jpg"),
    require("../assets/images/works/mineral.xi.3.4.jpg"),
    require("../assets/images/works/mineral.yu.9.16.jpg"),
  ],
  inkWash: [
    require("../assets/images/works/ink-wash.long.1.1.jpg"),
    require("../assets/images/works/ink-wash.jinfeng.9.16.jpg"),
    require("../assets/images/works/ink-wash.huashen.9.16.jpg"),
    require("../assets/images/works/ink-wash.qunshan.16.9.jpg"),
    require("../assets/images/works/ink-wash.heye.16.9.jpg"),
    require("../assets/images/works/ink-wash.he.16.9.jpg"),
    require("../assets/images/works/ink-wash.duizuo.16.9.jpg"),
    require("../assets/images/works/ink-wash.zuzi.16.9.jpg"),
    require("../assets/images/works/ink-wash.shuxia.9.16.jpg"),
    require("../assets/images/works/ink-wash.diaochan.9.16.jpg"),
    require("../assets/images/works/ink-wash.yuhuan.9.16.jpg"),
    require("../assets/images/works/ink-wash.xishi.9.16.jpg"),
    require("../assets/images/works/ink-wash.zhaojun.9.16.jpg"),
    require("../assets/images/works/ink-wash.ziyi.9.16.jpg"),
    require("../assets/images/works/ink-wash.nvhai.9.16.jpg"),
    require("../assets/images/works/ink-wash.wuti2.9.16.jpg"),
    require("../assets/images/works/ink-wash.wuti3.3.4.jpg"),
    require("../assets/images/works/ink-wash.xz.9.16.jpg"),
    require("../assets/images/works/ink-wash.wuti.3.4.jpg"),
    require("../assets/images/works/ink-wash.xiaonv.3.4.jpg"),
    require("../assets/images/works/ink-wash.yuanri.16.9.jpg"),
  ],
  photography: [
    require("../assets/images/works/photography.lan1.16.9.jpg"),
    require("../assets/images/works/photography.lan2.16.9.jpg"),
    require("../assets/images/works/photography.lan3.1.1.jpg"),
    require("../assets/images/works/photography.fly16.9.jpg"),
    require("../assets/images/works/photography.sky1.16.9.jpg"),
    require("../assets/images/works/photography.ta.16.9.jpg"),
    require("../assets/images/works/photography.yun.16.9.jpg"),
    require("../assets/images/works/photography.haibian5.16.9.jpg"),
    require("../assets/images/works/photography.haibian4.16.9.jpg"),
    require("../assets/images/works/photography.haibian3.16.9.jpg"),
    require("../assets/images/works/photography.haibian2.16.9.jpg"),
    require("../assets/images/works/photography.haibian1.16.9.jpg"),
    require("../assets/images/works/photography.bicycle.16.9.jpg"),
    require("../assets/images/works/photography.girls.16.9.jpg"),
    require("../assets/images/works/photography.beiying.16.9.jpg"),
    require("../assets/images/works/photography.sanren.9.16.jpg"),
    require("../assets/images/works/photography.run.9.16.jpg"),
    require("../assets/images/works/photography.daoying.16.9.jpg"),
    require("../assets/images/works/photography.guojin.16.9.jpg"),
    require("../assets/images/works/photography.light.16.9.jpg"),
    require("../assets/images/works/photography.light2.16.9.jpg"),
    require("../assets/images/works/photography.wuti.16.9.jpg"),
    require("../assets/images/works/photography.punk.16.9.jpg"),
    require("../assets/images/works/photography.orenge.16.9.jpg"),
    require("../assets/images/works/photography.zi.16.9.jpg"),
    require("../assets/images/works/photography.ju.16.9.jpg"),
    require("../assets/images/works/photography.fen.16.9.jpg"),
    require("../assets/images/works/photography.yellow.16.9.jpg"),
    require("../assets/images/works/photography.green.16.9.jpg"),
    require("../assets/images/works/photography.he.16.9.jpg"),
    require("../assets/images/works/photography.youle.16.9.jpg"),
    require("../assets/images/works/photography.xueren.9.16.jpg"),
    require("../assets/images/works/photography.youxiang.16.9.jpg"),
  ],
  other: {
    wallpaper: {
      mineral: require("../assets/images/works/wallpaper.mineral.jpg"),
      illustration: require("../assets/images/works/wallpaper.illustration.jpg"),
      inkWash: require("../assets/images/works/wallpaper.ink-wash.jpg"),
      photography: require("../assets/images/works/wallpaper.photo.jpg"),
    },
    scene: {
      mineral: require("../assets/images/works/scene.mineral.jpg"),
      illustrationLight: require("../assets/images/works/scene.illustration-light.jpg"),
      illustrationNight: require("../assets/images/works/scene.illustration-night.jpg"),
      inkWash: require("../assets/images/works/scene.ink-wash.jpg"),
      photography: require("../assets/images/works/scene.photo.jpg"),
      navigation: require("../assets/images/works/scene.mingming.jpg"),
    },
  },
};

// prepare image list which should be pre loaded before this app becomes available
const needLoad = [];
let total = 0;
Object.values(payload).forEach(function retrieve(a) {
  if (a instanceof Array) {
    total += a.length;
    a.forEach((b) => needLoad.push(b));
  } else if (typeof a === "string") {
    total++;
    needLoad.push(a);
  } else {
    Object.values(a).forEach(retrieve);
  }
});

export default {
  name: "Home",
  components: { Loading, KeanaBlog },
  data: () => ({
    version: "v1.9",
    completed: 0,
    total,
    cache: [],
    ...payload,
  }),

  computed: {
    progress() {
      return Math.ceil((this.completed / this.total) * 100);
    },
    ready() {
      return this.progress === 100;
    },
    info() {
      let action;
      switch (Math.ceil(this.progress / 10)) {
        case 0:
          action = "开始整理心情...";
          break;
        case 1:
        case 2:
        case 3:
          action = "正在翻找可以用的图片...";
          break;
        case 4:
        case 5:
        case 6:
          action = "正在驱赶捣乱的猫咪...";
          break;
        case 7:
        case 8:
        case 9:
          action = "正在准备饮料和甜点...";
          break;
        case 10:
          action = "一切准备就绪，开始进入...";
          break;
      }
      return `Version: ${this.version}, %progress%\n${action}`;
    },
  },

  mounted() {
    needLoad.forEach((b) => {
      const image = new Image();
      image.src = b;
      image.addEventListener("load", () => this.completed++);
      this.cache.push(image);
    });
  },
};
</script>
