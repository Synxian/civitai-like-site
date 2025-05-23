<template>
  <div
    class="mx-auto flex justify-center gap-4">
    <div
      class="flex max-w-[450px] flex-col gap-4 w-[320px]"
      v-for="col in columns"
      :key="col">
      <ImageDisplay
        v-for="image in imagesSplit(col, columns)"
        :key="image.id"
        :image-url="image.url"
        :image-id="image.id"
        :image-stats="image.stats" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import ImageDisplay from '../components/ImageDisplay.vue';
import Spinner from '../components/spinner.vue';
import { INFINITE_IMAGE_URL, IMAGE_URL, API_URL } from '../utils/constants';

export default {
  name: 'Images',
  components: {
    ImageDisplay,
    Spinner,
  },
  data() {
    return {
      images: [],
      cursor: 'null',
      loading: 0,
      columns: 5,
    };
  },
  computed: {
    requestInput() {
      const baseParams = `{"json":{"period": "Week","sort":"Most Reactions","types":["image"],"useIndex":true,"browsingLevel":1,"include":["cosmetics"],"excludedTagIds":[],"disablePoi":false,"disableMinor":false,"cursor":${this.cursor}}`
      return this.cursor === 'null' ? `${baseParams},"meta":{"values":{"cursor":["undefined"]}}}` : `${baseParams}}`
    },
    allImagesRequest() {
      return `${INFINITE_IMAGE_URL}?input=${this.requestInput}`
    },
  },
  created() {
    this.fetchImages();
    window.addEventListener("scroll", this.watchScroll);
    window.addEventListener('resize', this.handleResize);
  },
  methods: {
    async fetchImages() {
      try {
        const response = await axios.get(
          `${API_URL}/proxy`,
          {
            params: {
              url: this.allImagesRequest,
            },
          },
        );
        debugger
        this.images.push(...response.data.data.items.map(this.parseImage));
        this.cursor = response.data.result.data.json.nextCursor.split('|')[0];
      } catch (error) {
        console.error('Error fetching images:', error);
      }
    },
    parseImage(image) {
      return {
        id: image.id,
        url: `${IMAGE_URL}${image.url}/`,
        stats: image.stats,
      };
    },
    parseStats(stats) {
      return {
        likes: stats.likeCountAllTime,
        laughs: stats.laughCountAllTime,
        hearts: stats.heartCountAllTime,
        cries: stats.cryCountAllTime,
        tips: stats.tippedAmountCountAllTime,
      };
    },
    imagesSplit(col, columns) {
      return this.images.filter((_, index) => index % (columns + 1) === col);
    },
    watchScroll (event) {
      if (window.innerHeight + window.scrollY >= document.body.offsetHeight - 500) {
        this.fetchImages()
      }
    },
    handleResize() {
      this.columns = Math.ceil(window.innerWidth / 450);
    }
  },
};
</script>
