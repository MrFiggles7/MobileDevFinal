<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Go To Photos Page</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Tab 3</ion-title>
        </ion-toolbar>
      </ion-header>
      
      <ion-grid class="ion-align-items-center" style="height: 100%; display: block; vertical-align: middle">
        <ion-row size="12" class="ion-text-center" style="height: 100%">
          <ion-col style="height: 95%">
              <swiper style="height: 100%">
                <swiper-slide v-for="photo in photos" :key="photo" style="height: 100%">
                  <ion-img :src="photo.webviewPath" @click="showActionSheet(photo)"></ion-img>
                </swiper-slide>
              </swiper>
          </ion-col>
        </ion-row>
      </ion-grid>
      <ion-button
          href="/tabs/tab2"
          color="dark"
          vertical="bottom"
          horizontal="center"
          style="font-size: 2rem; display: flex; position: absolute; bottom: 5%; left: 0; right: 0;">
          Go To Photos
      </ion-button>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import '@ionic/vue/css/ionic-swiper.css';


import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  actionSheetController
} from '@ionic/vue';
import {usePhotoGallery, UserPhoto} from "@/composables/usePhotoGallery";
import {camera, close, trash} from "ionicons/icons";

export default defineComponent({
  name: 'Tab3Page',
  components: {
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
    Swiper,
    SwiperSlide,
  },

  setup() {
    const { photos, takePhoto, deletePhoto } = usePhotoGallery();

    const slideOpts = {
      initialSlide: 1,
      speed: 400
    };

    const showActionSheet = async (photo: UserPhoto) => {
      const actionSheet = await actionSheetController.create({
        header: 'Photos',
        buttons: [
          {
            text: 'Delete',
            role: 'destructive',
            icon: trash,
            handler: () => {
              deletePhoto(photo);
            },
          },
          {
            text: 'Cancel',
            icon: close,
            role: 'cancel',
            handler: () => {
              // Nothing to do, action sheet is automatically closed
            },
          },
        ],
      });
      await actionSheet.present();
    };

    return {
      photos,
      takePhoto,
      showActionSheet,
      camera,
      trash,
      close,
      slideOpts
    };
  },
});
</script>
