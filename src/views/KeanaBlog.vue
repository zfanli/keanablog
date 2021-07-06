<template>
  <div
    class="anime-demo"
    :style="{
      '--image-width': banner.imageWidth + 'px',
      '--image-height': banner.imageHeight + 'px',
      '--image-padding': banner.imagePadding + 'px',
    }"
  >
    <div class="scene banner-wrapper" ref="bannerWrapper">
      <div class="banner" ref="imageWrapper">
        <div class="image-frame" v-for="n in 9" :key="n"></div>
      </div>

      <div class="logo" :style="`width: ${logoWidth}px;`">
        <div
          class="logo-text"
          :style="`font-size: ${banner.imageWidth * 0.05}px;`"
        >
          <p>KEANA</p>
          <p>BLOG</p>
        </div>
        <div class="bio">
          music, milk tea and moods <br />
          necessary stuffs for creation
        </div>

        <div class="scroll-indicator">
          <v-icon class="scroll-icon">mdi-chevron-double-down</v-icon>
        </div>
      </div>
    </div>

    <div ref="main">
      <div id="illustration" class="scene">
        <div
          class="scene title"
          :style="{ 'background-image': `url(${scene.scene.illustration}` }"
        ></div>
        <gallery
          progressColor="pink"
          :images="illus"
          :style="{ 'background-image': `url(${scene.wallpaper.illustration}` }"
        ></gallery>
      </div>
      <div id="mineral" class="scene">
        <div
          class="scene title"
          :style="{ 'background-image': `url(${scene.scene.mineral}` }"
        ></div>
        <gallery
          progressColor="green"
          :images="mine"
          :style="{ 'background-image': `url(${scene.wallpaper.mineral}` }"
        ></gallery>
      </div>
      <div id="ink-wash" class="scene">
        <div
          class="scene title"
          :style="{ 'background-image': `url(${scene.scene.inkWash}` }"
        ></div>
        <gallery
          progressColor="yellow"
          :images="wash"
          :style="{ 'background-image': `url(${scene.wallpaper.inkWash}` }"
        ></gallery>
      </div>
      <div id="photography" class="scene">
        <div
          class="scene title"
          :style="{ 'background-image': `url(${scene.scene.photography}` }"
        ></div>
        <gallery
          progressColor="blue"
          :images="photo"
          :style="{ 'background-image': `url(${scene.wallpaper.photography}` }"
        ></gallery>
      </div>
    </div>

    <footer>Keana Blog, <br />made with Vue.js and animated with GSAP.</footer>

    <custom-nav :show="showNav" :active="navActive"></custom-nav>
  </div>
</template>
<script>
import { gsap } from "gsap";
import { ScrollTrigger } from "gsap/ScrollTrigger";
import { debounce } from "lodash";

import CustomNav from "../components/CustomNav.vue";
import Gallery from "../components/Gallery.vue";

gsap.registerPlugin(ScrollTrigger);
ScrollTrigger.defaults({
  start: "top top",
  scrub: 1,
});

export default {
  name: "KeanaBlog",
  components: { CustomNav, Gallery },

  props: {
    illustration: { type: Array, default: () => [] },
    mineral: { type: Array, default: () => [] },
    inkWash: { type: Array, default: () => [] },
    photography: { type: Array, default: () => [] },
    scene: { type: Object, default: () => {} },
  },

  data: () => {
    return {
      banner: {
        imageWidth: 0,
        imageHeight: 0,
        imagePadding: 0,
      },
      timeline: null,
      scrollTrigger: null,
      duration: 0.2,
      delay: 0,
      showNav: false,
      navActive: "",
    };
  },

  computed: {
    /**
     * Keep the aspect ratio of the logo area with the image width.
     */
    logoWidth() {
      const width = this.banner.imageWidth * 0.3;
      return window.innerWidth < width ? window.innerWidth : width;
    },

    illus() {
      return this.illustration.map((a) => this.parse(a));
    },

    mine() {
      return this.mineral.map((a) => this.parse(a));
    },

    wash() {
      return this.inkWash.map((a) => this.parse(a));
    },

    photo() {
      return this.photography.map((a) => this.parse(a));
    },
  },

  mounted() {
    // Bind resize handler after elements mounted.
    this.onResize();
    window.addEventListener(
      "resize",
      debounce(this.onResize, 250, { tailing: true })
    );

    // initialize scroll trigger after element rendered
    const init = () => {
      if (this.banner.imageWidth === 0) {
        setTimeout(init, 100);
        return;
      }

      // Pause banner animation due to the performance issue,
      // only play the animation when it's inside the visible area.
      const { main } = this.$refs;
      ScrollTrigger.create({
        trigger: main,
        onEnter: () => {
          this.timeline.pause();
          this.showNav = true;
        },
        onLeaveBack: () => {
          this.timeline.resume();
          this.showNav = false;
        },
      });

      ScrollTrigger.create({
        trigger: "#illustration",
        onEnter: () => (this.navActive = "插画"),
        onEnterBack: () => (this.navActive = "插画"),
      });

      ScrollTrigger.create({
        trigger: "#mineral",
        onEnter: () => (this.navActive = "岩彩"),
        onEnterBack: () => (this.navActive = "岩彩"),
      });

      ScrollTrigger.create({
        trigger: "#ink-wash",
        onEnter: () => (this.navActive = "水墨"),
        onEnterBack: () => (this.navActive = "水墨"),
      });

      ScrollTrigger.create({
        trigger: "#photography",
        onEnter: () => (this.navActive = "摄影"),
        onEnterBack: () => (this.navActive = "摄影"),
      });

      gsap.utils.toArray(".scene.title").forEach((a) =>
        ScrollTrigger.create({
          animation: gsap.to(a, { yPercent: -20 }),
          trigger: a,
          pin: true,
          pinSpacing: false,
          scrub: 0,
        })
      );
    };

    setTimeout(init, 0);
  },

  destroyed() {
    window.removeEventListener("resize", this.onResize);
  },

  methods: {
    /**
     * Start animation.
     */
    start() {
      const { bannerWrapper, imageWrapper } = this.$refs;
      const { duration, delay } = this;

      this.timeline = gsap
        .timeline()
        // mimic a gif but with transparency transition
        .fromTo(
          imageWrapper.children,
          { opacity: 0 },
          {
            opacity: 1,
            duration,
            delay,
            repeat: -1,
            yoyo: true,
            stagger: {
              each: duration,
            },
          }
        )
        // animate scroll behavior
        .fromTo(
          imageWrapper,
          { x: 0 },
          { id: "scrollAnimation", x: -1 * this.banner.imagePadding }
        );

      this.scrollTrigger = ScrollTrigger.create({
        animation: this.timeline.getById("scrollAnimation"),
        trigger: bannerWrapper,
        start: "top top",
        end: "+=" + this.banner.imagePadding,
        pin: true,
      });

      // Pause animation if banner is out of the current view.
      if (window.pageYOffset > imageWrapper.offsetHeight) {
        this.timeline.pause();
      }
    },

    /**
     * Kill the current animation to prepare for the next one.
     */
    clear() {
      this.timeline && this.timeline.kill();
      this.scrollTrigger && this.scrollTrigger.kill();
    },

    /**
     * Re-calculate the width and height of images and re-generate animation.
     */
    onResize() {
      // Re-calculate the width and height.
      const { clientHeight, clientWidth } = this.$refs.bannerWrapper;
      const { mobile } = this.$vuetify.breakpoint;
      const imageWidth =
          clientHeight / clientWidth <= 0.5626
            ? clientWidth
            : clientHeight * (16 / 9),
        imageHeight =
          clientHeight / clientWidth >= 0.5626
            ? clientHeight
            : clientWidth * (9 / 16) - 1,
        imagePadding = Math.max(0, imageWidth - clientWidth);

      // try again latter if element is not rendered
      if (imageWidth === 0) {
        setTimeout(() => this.onResize(), 100);
        return;
      }

      // Check and skip animation re-generation if the width is not changed on mobile devices,
      // to prevent unnecessary re-generation on mobile devices triggered by hiding address bar.
      if (!mobile || imageWidth !== this.banner.imageWidth) {
        this.banner.imageWidth = imageWidth;
        this.banner.imageHeight = imageHeight;
        this.banner.imagePadding = imagePadding;
        // Kill current animation.
        this.clear();
        // Re-generate the animation.
        this.start();
      }
    },

    parse(a) {
      const temp = a.split(".");
      let span;
      switch (`${temp[temp.length - 4]}:${temp[temp.length - 3]}`) {
        case "16:9":
          span = 4;
          break;
        case "4:3":
          span = 3;
          break;
        case "1:1":
        case "3:4":
          span = 2;
          break;
        case "9:16":
        default:
          span = 1;
          break;
      }
      return {
        src: a,
        span,
      };
    },
  },
};
</script>

<style lang="scss" scoped>
.anime-demo {
  .scene {
    width: 100%;
    min-height: 100vh;
    overflow: hidden;
    position: relative;
    background-size: cover;
    background-position: center;

    .banner {
      position: relative;
      height: 100vh;
      max-height: 100%;
      width: var(--image-width);
      overflow: hidden;
      background-image: url(../assets/images/banner/b1.jpg);
      background-size: 100%;

      .image-frame {
        background-size: 100%;
        max-height: 100%;
        right: 0;
        top: 0;
        position: absolute;
        width: var(--image-width);
        height: var(--image-height);
        opacity: 0;

        // generate image urls for all frames
        @for $i from 1 through 9 {
          $img-no: $i + 1;
          &:nth-child(#{$i}) {
            background-image: url(../assets/images/banner/b#{$img-no}.jpg);
          }
        }
      }
    }

    .logo {
      position: absolute;
      top: 0;
      right: 0;
      height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;

      .logo-text {
        color: rgb(220, 185, 155);
        font-family: "Rock Salt", cursive;
        background-image: url(../assets/images/banner/jj.png);
        background-size: contain;
        background-position: 50%;
        text-shadow: 0 0 10px rgb(0 0 0 / 30%);
      }

      .bio {
        color: rgba(255, 255, 255, 0.3);
      }

      .scroll-indicator {
        margin-top: 1rem;
        color: rgba(255, 255, 255, 0.8);
        position: relative;

        .scroll-icon {
          color: inherit;
          animation: float 6s ease-in-out infinite;
          position: relative;
        }
      }
    }
  }

  footer {
    height: 150px;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    text-align: center;
    vertical-align: bottom;
    padding-bottom: 2rem;
    background-color: #6f7a4c;
    color: rgba(255, 255, 255, 0.5);
  }
}

@keyframes float {
  0% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(-10px);
  }
  100% {
    transform: translateY(0px);
  }
}
</style>
