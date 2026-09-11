<template>
  <ion-card>
    <ion-card-header>
      <ion-card-title>Camera</ion-card-title>
    </ion-card-header>
    <ion-card-content>
      <ion-button expand="block" @click="takePicture">
        <ion-icon slot="start" :icon="cameraIcon" /> Take Picture
      </ion-button>
      <ion-text v-if="errorMessage" color="danger">
        <p>{{ errorMessage }}</p>
      </ion-text>
    </ion-card-content>
  </ion-card>
</template>

<script setup lang="ts">
import { IonCard, IonCardHeader, IonCardTitle, IonCardContent, IonButton, IonIcon, IonText } from "@ionic/vue";
import { camera as cameraIcon } from "ionicons/icons";
import { Camera } from "@capacitor/camera";
import { ref } from "vue";

const errorMessage = ref("");

// Fixed to match @photo-captured in your main page
const emit = defineEmits<{ (e: "photo-captured", photo: string): void; }>();

const takePicture = async () => {
  errorMessage.value = "";
  try {
    const photo = await Camera.takePhoto({ quality: 90, saveToGallery: false });
    if (photo.webPath) {
      emit("photo-captured", photo.webPath);
    }
  } catch (error) {
    errorMessage.value = "Error taking picture: " + error;
  }
};
</script>