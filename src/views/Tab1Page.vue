<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Card Photo Page</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large" >Tab 1</ion-title>
        </ion-toolbar>
      </ion-header>

      <ion-grid class="ion-align-items-center" style="height: 100%; display: block; vertical-align: middle">
        <ion-row size="12" class="ion-text-center" style="height: 100%">
          <ion-col v-for="photo in photos" :key="photo">
            <ion-card >
              <ion-card-header>
                <ion-card-subtitle>File Location: {{ photo.filepath }}</ion-card-subtitle>
                <ion-card-title>Local Storage Photo {{photos.indexOf(photo) + 1}}</ion-card-title>
              </ion-card-header>

              <ion-card-content>
                <ion-img :src="photo.webviewPath" @click="showActionSheet(photo)"></ion-img>
              </ion-card-content>
            </ion-card>

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
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonCard,
  IonCardContent,
  IonCardHeader,
  IonCardSubtitle,
  IonCardTitle, actionSheetController,
} from '@ionic/vue';
import {usePhotoGallery, UserPhoto} from "@/composables/usePhotoGallery";
import {camera, close, trash} from "ionicons/icons";

export default  defineComponent({
  name: 'Tab1Page',
  components: {
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
    IonCard,
    IonCardContent,
    IonCardHeader,
    IonCardSubtitle,
    IonCardTitle,
  },

  setup() {
    const { photos, takePhoto, deletePhoto } = usePhotoGallery();


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
    };
  },

});
</script>
