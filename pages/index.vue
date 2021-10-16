<template>
  <div class="container">
    <div
      class="text-center"
      v-if="screenshots.mobile && screenshots.tablet && screenshots.desktop"
    >
      <div id="framed-screenshots" class="flex justify-center">
        <cld-image class="self-start" :public-id="templates.mobile" width="400">
          <cld-transformation
            :overlay="screenshots.mobile.public_id.replaceAll('/', ':')"
            width="380"
            y="-20"
          />
        </cld-image>
        <cld-image class="self-start" :public-id="templates.tablet" width="400">
          <cld-transformation
            :overlay="screenshots.tablet.public_id.replaceAll('/', ':')"
            width="700"
          />
        </cld-image>
        <cld-image
          class="self-start"
          :public-id="templates.desktop"
          width="400"
        >
          <cld-transformation
            :overlay="screenshots.desktop.public_id.replaceAll('/', ':')"
            width="2550"
            gravity="north"
            y="125"
          />
        </cld-image>
      </div>
      <button
        type="button"
        class="
          inline-flex
          items-center
          px-4
          py-2
          border border-transparent
          text-sm
          font-medium
          rounded-md
          text-indigo-700
          bg-indigo-100
          hover:bg-indigo-200
          focus:outline-none
          focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500
        "
        @click="download"
        v-if="framedScreenshots.length < 1"
      >
        Download
      </button>
      <a
        v-for="(screenshot, index) in framedScreenshots"
        :key="index"
        :href="screenshot"
        target="_blank"
        class="block text-sm font-medium p-2"
      >
        Screenshot No.{{ index + 1 }}
      </a>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      uploading: false,
      url: "https://google.com",
      templates: {
        mobile: "nuxtjs-website-screenshot-framer/templates/iphone.png",
        tablet: "nuxtjs-website-screenshot-framer/templates/ipad.png",
        desktop: "nuxtjs-website-screenshot-framer/templates/MacBook.png",
      },
      sizes: {
        mobile: {
          width: 750,
          height: 1334,
        },
        tablet: {
          width: 1620,
          height: 2160,
        },
        desktop: {
          width: 1920,
          height: 1080,
        },
      },
      screenshots: {
        mobile: null,
        tablet: null,
        desktop: null,
      },
      framedScreenshots: [],
    };
  },
  computed: {
    mobileScreenshotUrl() {
      let url = `https://api.apiflash.com/v1/urltoimage?access_key=${process.env.NUXT_ENV_API_FLASH_KEY}`;
      url += `&height=${this.sizes.mobile.height}`;
      url += `&url=${this.url}`;
      url += `&width=${this.sizes.mobile.width}`;
      return url;
    },
    tabletScreenshotUrl() {
      let url = `https://api.apiflash.com/v1/urltoimage?access_key=${process.env.NUXT_ENV_API_FLASH_KEY}`;
      url += `&height=${this.sizes.tablet.height}`;
      url += `&url=${this.url}`;
      url += `&width=${this.sizes.tablet.width}`;
      return url;
    },
    desktopScreenshotUrl() {
      let url = `https://api.apiflash.com/v1/urltoimage?access_key=${process.env.NUXT_ENV_API_FLASH_KEY}`;
      url += `&height=${this.sizes.desktop.height}`;
      url += `&url=${this.url}`;
      url += `&width=${this.sizes.desktop.width}`;
      return url;
    },
    domainName() {
      return this.url
        .replace("http://", "")
        .replace("https://", "")
        .replace("www", "")
        .split("/")[0];
    },
  },
  methods: {
    async uploadScreenshots(e) {
      this.uploading = true;
      this.toDataURL(
        this.mobileScreenshotUrl,
        async (dataUrl) =>
          (this.screenshots.mobile = await this.$cloudinary.upload(dataUrl, {
            public_id: "mobile",
            upload_preset: "default-preset",
            folder: `nuxtjs-website-screenshot-framer/${this.domainName}`,
          }))
      );
      this.toDataURL(
        this.tabletScreenshotUrl,
        async (dataUrl) =>
          (this.screenshots.tablet = await this.$cloudinary.upload(dataUrl, {
            public_id: "tablet",
            upload_preset: "default-preset",
            folder: `nuxtjs-website-screenshot-framer/${this.domainName}`,
          }))
      );
      this.toDataURL(
        this.desktopScreenshotUrl,
        async (dataUrl) =>
          (this.screenshots.desktop = await this.$cloudinary.upload(dataUrl, {
            public_id: "desktop",
            upload_preset: "default-preset",
            folder: `nuxtjs-website-screenshot-framer/${this.domainName}`,
          }))
      );
      this.uploading = false;
    },
    toDataURL(url, callback) {
      var xhr = new XMLHttpRequest();
      xhr.onload = function () {
        var reader = new FileReader();
        reader.onloadend = function () {
          callback(reader.result);
        };
        reader.readAsDataURL(xhr.response);
      };
      xhr.open("GET", url);
      xhr.responseType = "blob";
      xhr.send();
    },
    download() {
      const container = document.getElementById("framed-screenshots");
      const images = container.getElementsByTagName("img");
      for (let image of images) {
        this.framedScreenshots.push(image.src);
      }
      console.log(this.framedScreenshots);
    },
  },
};
</script>
