<template>
  <div class="grid grid-cols-3 m-10">
    <cld-image :public-id="templates.mobile" width="400">
      <cld-transformation
        overlay="nuxtjs-website-screenshot-framer:google.com:mobile"
        width="380"
        y="-20"
      />
    </cld-image>
    <cld-image :public-id="templates.tablet" width="400">
      <cld-transformation
        overlay="nuxtjs-website-screenshot-framer:google.com:tablet"
        width="700"
      />
    </cld-image>
    <cld-image :public-id="templates.desktop" width="400">
      <cld-transformation
        overlay="nuxtjs-website-screenshot-framer:google.com:desktop"
        width="2550"
        gravity="north"
        y="125"
      />
    </cld-image>
  </div>
</template>

<script>
export default {
  data() {
    return {
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
        mobile: "nuxtjs-website-screenshot-framer/google.com/mobile",
        tablet: "nuxtjs-website-screenshot-framer/google.com/tablet",
        desktop: "nuxtjs-website-screenshot-framer/google.com/desktop",
      },
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
  },
};
</script>
