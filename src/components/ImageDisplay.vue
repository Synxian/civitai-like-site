<template>
  <Spinner v-if="loading" />
  <div v-else class="flex flex-col rounded-md border-gray-300 bg-gray-100 shadow-gray-400 border dark:border-gray-800 dark:bg-gray-900 dark:shadow-gray-950">
    <img :src="image" alt="Image" class="rounded-t-md" />
    <ReactionBar
      :likes="imageStats.likeCountAllTime"
      :laughs="imageStats.laughCountAllTime"
      :hearts="imageStats.heartCountAllTime"
      :cries="imageStats.cryCountAllTime"
      :tippedAmount="imageStats.tippedAmountCountAllTime" />
  </div>
</template>

<script>
import Spinner from './spinner.vue';
import ReactionBar from './reactionBar.vue';
import { PROXY_URL } from '../utils/constants';
const IMAGE_PARAMS = 'anim=false,width=450/'

export default {
  name: 'ImageDisplay',
  components: {
    Spinner,
    ReactionBar,
  },
  props: {
    imageUrl: {
      type: String,
      required: true,
    },
    imageId: {
      type: Number,
      required: true,
    },
    imageStats: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      image: null,
      loading: 0,
    }
  },
  created() {
    this.getImage();
  },
  methods: {
    async getImage() {
      try {
        this.loading++;
        const response = await fetch(`${PROXY_URL}${this.imageUrl}${IMAGE_PARAMS}${this.imageId}`);
        const blob = await response.blob();
        this.image = URL.createObjectURL(blob);
      } catch (error) {
        console.error('Error fetching image:', error);
      } finally {
        this.loading--;
      }
    },
  },
}
</script>