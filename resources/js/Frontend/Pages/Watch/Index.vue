<script>
import { Link, Head } from '@inertiajs/inertia-vue3'
import Share from '../../Shared/Share.vue'
import VideoPlayer from '../../Shared/VideoPlayer.vue'

import 'magnific-popup/dist/jquery.magnific-popup'
import 'magnific-popup/dist/magnific-popup.css'

export default {
  components: {
    Share,
    Head,
    VideoPlayer,
    Link,
  },
  props: {
    movie: Object,
  },
  data() {
    return {
      videoOptions: {
        type: 'video',
        title: this.movie.title,
        poster: this.movie.backdrop_path,
        previewThumbnails: {
          enabled: false,
          src: this.movie.backdrop_path,
        },
        sources: [
          {
            src: '',
            type: 'video/mp4',
            size: 1080,
          },
        ],
      },
    }
  },
  methods: {
    runtimeToHours(runtime) {
      const hours = Math.floor(runtime / 60)
      const minutes = runtime % 60
      return `${hours}h ${minutes}m`
    },
    handleScroll() {
      if (window.scrollY > 0) {
        document.querySelector('header').classList.add('header--scroll')
      } else {
        document.querySelector('header').classList.remove('header--scroll')
      }
    },
  },
  mounted() {
    $('.open-video').magnificPopup({
      fixedContentPos: true,
      showCloseBtn: false,
      type: 'iframe',
      preloader: false,
      removalDelay: 300,
      mainClass: 'mfp-fade',
    })

    document.querySelector(
      '.section__bg'
    ).style.backgroundImage = `url(${this.movie.backdrop_path})`
  },
  created() {
    window.addEventListener('scroll', this.handleScroll)
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.handleScroll)
  },
}
</script>

<template>
  <Head :title="movie.title" />
  <section
    class="
      section
      section--head
      section--head-fixed
      section--gradient
      section--details-bg
    "
  >
    <div class="section__bg"></div>
    <div class="container">
      <div class="article">
        <div class="row">
          <div class="col-12 col-xl-8">
            <Link
              v-if="movie.videos.results[0]"
              :href="`https://www.youtube.com/watch?v=${movie.videos.results[0].key}`"
              class="article__trailer open-video"
            >
              <svg
                width="22"
                height="22"
                viewBox="0 0 22 22"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill-rule="evenodd"
                  clip-rule="evenodd"
                  d="M11 1C16.5228 1 21 5.47716 21 11C21 16.5228 16.5228 21 11 21C5.47716 21 1 16.5228 1 11C1 5.47716 5.47716 1 11 1Z"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                ></path>
                <path
                  fill-rule="evenodd"
                  clip-rule="evenodd"
                  d="M14.0501 11.4669C13.3211 12.2529 11.3371 13.5829 10.3221 14.0099C10.1601 14.0779 9.74711 14.2219 9.65811 14.2239C9.46911 14.2299 9.28711 14.1239 9.19911 13.9539C9.16511 13.8879 9.06511 13.4569 9.03311 13.2649C8.93811 12.6809 8.88911 11.7739 8.89011 10.8619C8.88911 9.90489 8.94211 8.95489 9.04811 8.37689C9.07611 8.22089 9.15811 7.86189 9.18211 7.80389C9.22711 7.69589 9.30911 7.61089 9.40811 7.55789C9.48411 7.51689 9.57111 7.49489 9.65811 7.49789C9.74711 7.49989 10.1091 7.62689 10.2331 7.67589C11.2111 8.05589 13.2801 9.43389 14.0401 10.2439C14.1081 10.3169 14.2951 10.5129 14.3261 10.5529C14.3971 10.6429 14.4321 10.7519 14.4321 10.8619C14.4321 10.9639 14.4011 11.0679 14.3371 11.1549C14.3041 11.1999 14.1131 11.3999 14.0501 11.4669Z"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                ></path>
              </svg>
              Trailer
            </Link>
            <div class="article__content">
              <h1>{{ movie.title }}</h1>
              <ul class="list">
                <li>
                  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                    <path
                      d="M22,9.67A1,1,0,0,0,21.14,9l-5.69-.83L12.9,3a1,1,0,0,0-1.8,0L8.55,8.16,2.86,9a1,1,0,0,0-.81.68,1,1,0,0,0,.25,1l4.13,4-1,5.68A1,1,0,0,0,6.9,21.44L12,18.77l5.1,2.67a.93.93,0,0,0,.46.12,1,1,0,0,0,.59-.19,1,1,0,0,0,.4-1l-1-5.68,4.13-4A1,1,0,0,0,22,9.67Zm-6.15,4a1,1,0,0,0-.29.88l.72,4.2-3.76-2a1.06,1.06,0,0,0-.94,0l-3.76,2,.72-4.2a1,1,0,0,0-.29-.88l-3-3,4.21-.61a1,1,0,0,0,.76-.55L12,5.7l1.88,3.82a1,1,0,0,0,.76.55l4.21.61Z"
                    />
                  </svg>
                  {{ movie.vote_average }}
                </li>
                <li>{{ movie.genres[0].name }}</li>
                <li>{{ movie.release_date }}</li>
                <li>{{ runtimeToHours(movie.runtime) }}</li>
                <li>16+</li>
              </ul>

              <p>
                {{ movie.overview }}
              </p>
            </div>
          </div>
          <div class="col-12 col-xl-8">
            <VideoPlayer :options="videoOptions" />
            <div class="article__actions article__actions--details">
              <div class="article__download">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                  <path
                    d="M21,14a1,1,0,0,0-1,1v4a1,1,0,0,1-1,1H5a1,1,0,0,1-1-1V15a1,1,0,0,0-2,0v4a3,3,0,0,0,3,3H19a3,3,0,0,0,3-3V15A1,1,0,0,0,21,14Zm-9.71,1.71a1,1,0,0,0,.33.21.94.94,0,0,0,.76,0,1,1,0,0,0,.33-.21l4-4a1,1,0,0,0-1.42-1.42L13,12.59V3a1,1,0,0,0-2,0v9.59l-2.29-2.3a1,1,0,1,0-1.42,1.42Z"
                  />
                </svg>
                Tải về:
                <a href="#" download="#">480p</a>
                <a href="#" download="#">720p</a>
                <a href="#" download="#">1080p</a>
                <a href="#" download="#">4k</a>
              </div>

              <button class="article__favorites" type="button">
                <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
                  <path
                    d="M16,2H8A3,3,0,0,0,5,5V21a1,1,0,0,0,.5.87,1,1,0,0,0,1,0L12,18.69l5.5,3.18A1,1,0,0,0,18,22a1,1,0,0,0,.5-.13A1,1,0,0,0,19,21V5A3,3,0,0,0,16,2Zm1,17.27-4.5-2.6a1,1,0,0,0-1,0L7,19.27V5A1,1,0,0,1,8,4h8a1,1,0,0,1,1,1Z"
                  ></path>
                </svg>
                Thêm vào danh sách yêu thích
              </button>
            </div>
          </div>
          <div class="col-12 col-xl-8">
            <div class="categories">
              <h3 class="categories__title">Thể loại</h3>
              <Link
                href="/"
                class="categories__item"
                v-for="genre in movie.genres"
                :key="genre.id"
              >
                {{ genre.name }}
              </Link>
            </div>
            <Share />
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

