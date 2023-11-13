<template>
    <header-app :title="movie.title" />
    <ion-content class="detail-container">
        <ion-row>
            <ion-col>
                <ion-img :src="`${BASE_IMG}${movie.backdrop_path}`" />
                <p class="text-center">{{ movie.release_date }}</p>
            </ion-col>
        </ion-row>
        <ion-row class="d-flex justify-content-center detail-container__genres">
            <ion-col size="12" class="d-flex justify-content-center">
                <ion-chip class="detail-container__genres__chip" color="primary" size="small" v-for="genre in movie.genres">{{ genre.name }}</ion-chip>
            </ion-col>
        </ion-row>
        <ion-row class="d-flex justify-content-center detail-container__description">
            <ion-col size="12">
                <h5 class="detail-container__description__title">Resumen</h5>
                <p class="detail-container__description__text">
                    {{ movie.overview }}
                </p>
            </ion-col>
        </ion-row>
        <ion-row class="d-flex justify-content-center detail-container__companies">
            <ion-col size="12" class="d-flex justify-content-center">
                <div v-for="company in movie.production_companies">
                    <ion-img v-if="company.logo_path" :src="`${BASE_IMG}${company.logo_path}`" class="detail-container__companies__image" />
                </div>
            </ion-col>
        </ion-row>
        <ion-row  class="d-flex justify-content-center detail-container__trailer">
            <ion-col size="12">
                <ion-button @click="openTrailer = true" expand="block" fill="outline">Ver Trailer</ion-button>
            </ion-col>
        </ion-row>
        <!--Modal Trailer-->
        <ion-modal :is-open="openTrailer">
            <ion-header>
                <ion-toolbar>
                    <ion-buttons slot="start">
                        <ion-button @click="openTrailer = false">Cerrar</ion-button>
                    </ion-buttons>
                </ion-toolbar>
            </ion-header>
            <ion-content class="ion-padding">
                <ion-title class="modal-title">
                    {{ movie.title }}
                </ion-title>
                <ion-row>
                    <ion-col>
                        <iframe width="100%" height="auto" :src="`https://www.youtube.com/embed/${trailer}`" 
                        :title="movie.title" 
                        frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen>
                        </iframe>
                    </ion-col>
                </ion-row>
            </ion-content>
        </ion-modal>
    </ion-content>
</template>

<script lang="ts">
import MovieServices from "@/services/MovieServices"
import { useRoute } from 'vue-router'
import HeaderApp from "@/components/HeaderApp.vue"
import { IonContent, IonRow, IonCol, IonImg, IonButton, IonButtons, IonModal, IonToolbar, IonTitle } from '@ionic/vue';
export default {
    name : 'MovieDetail',
    components: {
        HeaderApp,
        IonContent,
        IonRow,
        IonCol,
        IonImg,
        IonButton,
        IonButtons,
        IonModal,
        IonToolbar,
        IonTitle
    },
    data() {
        return {
            BASE_IMG : "https://image.tmdb.org/t/p/w500/",
            id: '',
            movie: {},
            videos: [],
            trailer: '',
            openTrailer: false
        }
    },
    async created() {
        const route = useRoute()
        this.id = route.params.id
        this.movie = await MovieServices.getMovieDetail(this.id)
        this.videos = await MovieServices.getMovieTrailer(this.id)
        if(this.videos.length > 0){
            this.trailer = this.videos[0].key
        }
    }
}

</script>

<style></style>