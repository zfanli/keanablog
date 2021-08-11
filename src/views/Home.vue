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

const bucketURL = "https://keanablog.oss-cn-shanghai.aliyuncs.com/",
  payload = {
    banner: [
      "b1.jpg",
      "b2.jpg",
      "b3.jpg",
      "b4.jpg",
      "b5.jpg",
      "b6.jpg",
      "b7.jpg",
      "b8.jpg",
      "b9.jpg",
      "b10.jpg",
      "jj.png",
    ],
    illustration: [
      "illustration.panda.9.16.jpg",
      "illustration.peach.9.16.jpg",
      "illustration.girls.16.9.jpg",
      "illustration.watermelon.3.4.jpg",
      "illustration.lemonade.3.4.jpg",
      "illustration.fruittea.3.4.jpg",
      "illustration.sunrise.4.3.jpg",
      "illustration.snow.3.4.jpg",
      "illustration.yue.4.3.jpg",
      "illustration.girl.9.16.jpg",
      "illustration.coffee.9.16.jpg",
      "illustration.coffee2.1.1.jpg",
      "illustration.whale.1.1.jpg",
      "illustration.three.16.9.jpg",
      "illustration.room.16.9.jpg",
      "illustration.forest.16.9.jpg",
      "illustration.wind.16.9.jpg",
      "illustration.kk.16.9.jpg",
      "illustration.three-1.9.16.jpg",
      "illustration.three-2.9.16.jpg",
      "illustration.three-3.9.16.jpg",
      "illustration.six.9.16.jpg",
      "illustration.ric.9.16.jpg",
      "illustration.f.3.4.jpg",
      "illustration.r.9.16.jpg",
    ],
    mineral: [
      "mineral.qing.16.9.jpg",
      "mineral.qin.16.9.jpg",
      "mineral.shuimu.16.9.jpg",
      "mineral.win.16.9.jpg",
      "mineral.fish.9.16.jpg",
      "mineral.fenghuo.9.16.jpg",
      "mineral.yan.16.9.jpg",
      "mineral.denglong.16.9.jpg",
      "mineral.zuma.16.9.jpg",
      "mineral.shanzi.3.4.jpg",
      "mineral.jiu.3.4.jpg",
      "mineral.kong.16.9.jpg",
      "mineral.linmo.3.4.jpg",
      "mineral.linmo2.3.4.jpg",
      "mineral.sakura4.3.jpg",
      "mineral.us.3.4.jpg",
      "mineral.use.3.4.jpg",
      "mineral.yinyue.9.16.jpg",
      "mineral.xi.3.4.jpg",
      "mineral.yu.9.16.jpg",
    ],
    inkWash: [
      "ink-wash.long.1.1.jpg",
      "ink-wash.jinfeng.9.16.jpg",
      "ink-wash.huashen.9.16.jpg",
      "ink-wash.wuti3.3.4.jpg",
      "ink-wash.qunshan.16.9.jpg",
      "ink-wash.heye.16.9.jpg",
      "ink-wash.he.16.9.jpg",
      "ink-wash.duizuo.16.9.jpg",
      "ink-wash.zuzi.16.9.jpg",
      "ink-wash.shuxia.9.16.jpg",
      "ink-wash.diaochan.9.16.jpg",
      "ink-wash.yuhuan.9.16.jpg",
      "ink-wash.xishi.9.16.jpg",
      "ink-wash.zhaojun.9.16.jpg",
      "ink-wash.ziyi.9.16.jpg",
      "ink-wash.nvhai.9.16.jpg",
      "ink-wash.wuti2.9.16.jpg",
      "ink-wash.yuanri.16.9.jpg",
      "ink-wash.xz.9.16.jpg",
      "ink-wash.wuti.3.4.jpg",
      "ink-wash.xiaonv.3.4.jpg",
    ],
    photography: [
      "photography.lan1.16.9.jpg",
      "photography.lan2.16.9.jpg",
      "photography.lan3.1.1.jpg",
      "photography.fly.1.1.jpg",
      "photography.sky1.16.9.jpg",
      "photography.ta.16.9.jpg",
      "photography.yun.16.9.jpg",
      "photography.haibian5.16.9.jpg",
      "photography.haibian4.16.9.jpg",
      "photography.haibian3.16.9.jpg",
      "photography.haibian2.16.9.jpg",
      "photography.haibian1.16.9.jpg",
      "photography.bicycle.16.9.jpg",
      "photography.girls.16.9.jpg",
      "photography.beiying.16.9.jpg",
      "photography.sanren.9.16.jpg",
      "photography.run.9.16.jpg",
      "photography.daoying.16.9.jpg",
      "photography.guojin.16.9.jpg",
      "photography.light.16.9.jpg",
      "photography.light2.16.9.jpg",
      "photography.wuti.16.9.jpg",
      "photography.punk.16.9.jpg",
      "photography.orenge.16.9.jpg",
      "photography.zi.16.9.jpg",
      "photography.ju.16.9.jpg",
      "photography.fen.16.9.jpg",
      "photography.yellow.16.9.jpg",
      "photography.green.16.9.jpg",
      "photography.he.16.9.jpg",
      "photography.youle.16.9.jpg",
      "photography.xueren.3.4.jpg",
      "photography.youxiang.3.4.jpg",
    ],
    other: {
      wallpaper: {
        mineral: "wallpaper.mineral.jpg",
        illustration: "wallpaper.illustration.jpg",
        inkWash: "wallpaper.ink-wash.jpg",
        photography: "wallpaper.photo.jpg",
      },
      // wallpaper: {
      //   mineral:
      //     "data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7",
      //   illustration:
      //     "data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7",
      //   inkWash:
      //     "data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7",
      //   photography:
      //     "data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7",
      // },
      scene: {
        mineral: "scene.mineral.jpg",
        illustrationLight: "scene.illustration-light.jpg",
        illustrationNight: "scene.illustration-night.jpg",
        inkWash: "scene.ink-wash.jpg",
        photography: "scene.photo.jpg",
        navigation: "scene.mingming.jpg",
      },
    },
  };

function makeURL(obj) {
  if (obj instanceof Array) {
    obj = obj.map((a) => bucketURL + a);
  } else if (typeof obj === "string") {
    if (!obj.startsWith("data:")) obj = bucketURL + obj;
  } else {
    Object.keys(obj).forEach((key) => {
      obj[key] = makeURL(obj[key]);
    });
  }
  return obj;
}

makeURL(payload);

// prepare image list which should be pre loaded before this app becomes available
const needLoad = [];
let total = 0;
Object.values(payload).forEach(function retrieve(a) {
  if (a instanceof Array) {
    total += a.length;
    a.forEach((b) => needLoad.push(b));
  } else if (typeof a === "string") {
    if (!a.startsWith("data:")) {
      total++;
      needLoad.push(a);
    }
  } else {
    Object.values(a).forEach(retrieve);
  }
});

export default {
  name: "Home",
  components: { Loading, KeanaBlog },
  data: () => ({
    // HEY! NOT FOUND THE VERSION NUMBER AGAIN?! SERIOUSLY?!!!
    // ITS HERE! LOOK AT ME!!!
    // OKAY I KNOW YOU CANT SEE ME IF I'M SHORT!!!
    // THERE ARE FOUR LINES HERE, DOES IT ENOUGH TO LET YOU SEE ME???
    version: "v1.16",
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
      if (b.startsWith("data:")) return;
      const image = new Image();
      image.src = b;
      image.addEventListener("load", () => this.completed++);
      this.cache.push(image);
    });
  },
};
</script>
