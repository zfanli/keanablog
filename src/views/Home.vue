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

const bucketURL = "https://keanablog.oss-cn-shanghai.aliyuncs.com/";

export default {
  name: "Home",
  components: { Loading, KeanaBlog },
  data: () => {
    const makeURL = (obj) => {
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
    };

    return {
      // HEY! NOT FOUND THE VERSION NUMBER AGAIN?! SERIOUSLY?!!!
      // ITS HERE! LOOK AT ME!!!
      // OKAY I KNOW YOU CANT SEE ME IF I'M SHORT!!!
      // THERE ARE FOUR LINES HERE, DOES IT ENOUGH TO LET YOU SEE ME???
      version: "v1.18",
      completed: 0,
      total: 0,
      cache: [],
      ...makeURL({
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
        illustration: [],
        mineral: [],
        inkWash: [],
        photography: [],
        other: {
          wallpaper: {
            mineral: "wallpaper.mineral.jpg",
            illustration: "wallpaper.illustration.jpg",
            inkWash: "wallpaper.ink-wash.jpg",
            photography: "wallpaper.photo.jpg",
          },
          scene: {
            mineral: "scene.mineral.jpg",
            illustrationLight: "scene.illustration-light.jpg",
            illustrationNight: "scene.illustration-night.jpg",
            inkWash: "scene.ink-wash.jpg",
            photography: "scene.photo.jpg",
            navigation: "scene.mingming.jpg",
          },
        },
      }),
    };
  },

  computed: {
    progress() {
      if (this.total === 0) return 0;
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

  async mounted() {
    this.illustration = await this.loadGallery(
      bucketURL + "images/gallery-illustrations.txt?" + Date.now(),
      "/api/gallery-illustrations.txt?" + Date.now()
    );
    this.mineral = await this.loadGallery(
      bucketURL + "images/gallery-mineral-color.txt?" + Date.now(),
      "/api/gallery-mineral-color.txt?" + Date.now()
    );
    this.inkWash = await this.loadGallery(
      bucketURL + "images/gallery-ink-wash-painting.txt?" + Date.now(),
      "/api/gallery-ink-wash-painting.txt?" + Date.now()
    );
    this.photography = await this.loadGallery(
      bucketURL + "images/gallery-photography.txt?" + Date.now(),
      "/api/gallery-photography.txt?" + Date.now()
    );

    // prepare image list which should be pre loaded before this app becomes available
    const needLoad = [];

    [this.other.wallpaper, this.other.scene].forEach((obj) => {
      Object.values(obj).forEach((img) => needLoad.push(img));
    });

    [this.illustration, this.mineral, this.inkWash, this.photography].forEach(
      (list) => {
        let count = 0;
        list.forEach((img) => {
          if (count > 15) return;
          needLoad.push(img);
        });
      }
    );

    this.total = needLoad.length;

    needLoad.forEach((b) => {
      if (b.startsWith("data:")) return;
      const image = new Image();
      image.src = b;
      image.addEventListener("load", () => this.completed++);
      this.cache.push(image);
    });
  },

  methods: {
    async loadGallery(api, fallback) {
      try {
        const res = await fetch(api, { method: "GET", mode: "cors" });
        const data = await res.text();
        const result = [];
        data.split(/\n/).forEach((line) => {
          line = line.trim();
          if (line === "<!DOCTYPE html>") throw new Error();
          if (line !== "") result.push(bucketURL + line);
        });
        return result;
      } catch {
        if (fallback) return this.loadGallery(fallback, null);
      }
    },
  },
};
</script>
