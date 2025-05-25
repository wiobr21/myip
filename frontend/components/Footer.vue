<template>
  <footer>
    <div id="copyleft" class="text-center">
      <!-- 包裹图片的跳转链接 -->
      <a href="https://wobshare.us.kg/" target="_blank">
        <img
          src="https://moc.wobshare.us.kg/521"
          alt="about-image"
          style="pointer-events: none; user-drag: none; -webkit-user-drag: none; width: 280px; height: 80px; margin-bottom: 10px;"
          draggable="false"
        />
      </a>
      <p>
        <span>Powered by 𝓌𝑜𝒷</span>
        <a :href="t('page.footerLink')"
           class="link-dark" target="_blank"
           @click="trackEvent('Footer', 'FooterClick', 'Github');"
           aria-label="Github">
          <i class="bi bi-github" :class="{ 'dark-mode': isDarkMode }"
             v-tooltip="{ title: t('Tooltips.GithubLink'), placement: 'top' }"></i>
        </a>
      </p>
    </div>
    
    <div id="about" class="text-center mb-2">
      <a
        class="link link-underline-offset link-underline-opacity-0"
        :class="[isDarkMode ? 'link-light' : 'link-dark']"
        role="button"
        aria-controls="About"
        @click.prevent="openAbout"
      >
      </a>
    </div>
  </footer>
</template>

<script setup>
import { ref, computed, reactive } from 'vue';
import { useMainStore } from '@/store';
import { Offcanvas } from 'bootstrap';
import { useI18n } from 'vue-i18n';
import { trackEvent } from '@/utils/use-analytics';

const { t, tm } = useI18n();

const store = useMainStore();
const isDarkMode = computed(() => store.isDarkMode);
const isMobile = computed(() => store.isMobile);
const configs = computed(() => store.configs);

const content = ref('about');
const showAbout = ref(true);
const showChangelog = ref(false);
const showSpecialThanks = ref(false);
const changelog = reactive(tm('changelog.versions'));

const thanksList = [
  {
    name: 'Setilis Hu',
    link: ''
  },
  {
    name: 'Seven Yu',
    link: 'https://github.com/dofy'
  },
  {
    name: 'Nikolai Tschacher',
    link: 'https://incolumitas.com/pages/about/'
  },
  {
    name: 'Project Alexandria (Cloudflare)',
    link: 'https://www.cloudflare.com/lp/project-alexandria/'
  },
  {
    name: 'Cloudflare Speedtest',
    link: 'https://github.com/cloudflare/speedtest'
  },
  {
    name: 'Globalping by jsDelivr',
    link: 'https://globalping.io/'
  },
  {
    name: 'ChatGPT',
    link: 'https://chatgpt.com/'
  }
]

const openAbout = () => {
  var offcanvasElement = document.getElementById('About');
  var offcanvas = Offcanvas.getInstance(offcanvasElement) || new Offcanvas(offcanvasElement);
  if (offcanvasElement.classList.contains('show')) {
    offcanvas.hide();
  } else {
    offcanvas.show();
  }

  trackEvent('Footer', 'FooterClick', 'About');

};

const offcanvasBody = ref(null);

const toggleContent = (contentType) => {
  showAbout.value = contentType === 'about';
  showChangelog.value = contentType === 'changelog';
  showSpecialThanks.value = contentType === 'specialthanks';
  content.value = contentType;
  offcanvasBody.scrollTop = 0;
};

defineExpose({
  openAbout
});
</script>

<style scoped>
#About {
  z-index: 1051;
}

.jn-placeholder {
  height: 20pt;
}

.jn-heart-color {
  color: red;
}
</style>
