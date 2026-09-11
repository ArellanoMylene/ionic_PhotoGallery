<template>
  <ion-card class="camera-card">
    <!-- Glowing Top Accent Line -->
    <div class="card-glow-bar"></div>

    <ion-card-header class="camera-header">
      <div class="header-left">
        <div class="live-indicator">
          <span class="live-dot"></span>
          <span class="live-pulse"></span>
        </div>
        <ion-card-title class="camera-title">Camera</ion-card-title>
      </div>
    </ion-card-header>

    <ion-card-content class="camera-content">
      <!-- Aesthetic Viewfinder Lens Deck -->
      <div class="viewfinder-deck">
        <!-- Lens Graphic Element -->
        <div class="lens-container">
          <div class="lens-ring outer"></div>
          <div class="lens-ring inner"></div>
          <div class="lens-glass">
            <ion-icon :icon="cameraIcon" class="lens-icon" />
          </div>
        </div>

        <!-- Viewfinder Target Frame -->
        <div class="corner corner-tl"></div>
        <div class="corner corner-tr"></div>
        <div class="corner corner-bl"></div>
        <div class="corner corner-br"></div>
        
        <p class="deck-hint">READY TO CAPTURE</p>
      </div>

      <!-- High-Glow Action Button -->
      <ion-button expand="block" class="capture-button" @click="takePicture">
        <div class="button-content">
          <ion-icon :icon="cameraIcon" class="btn-icon" />
          <span class="btn-text">Take Picture</span>
        </div>
      </ion-button>

      <!-- Glassmorphic Danger Error Card -->
      <transition name="fade">
        <div v-if="errorMessage" class="error-banner">
          <div class="error-chip">!</div>
          <p class="error-text">{{ errorMessage }}</p>
        </div>
      </transition>
    </ion-card-content>
  </ion-card>
</template>

<script setup lang="ts">
import { IonCard, IonCardHeader, IonCardTitle, IonCardContent, IonButton, IonIcon } from "@ionic/vue";
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

<style scoped>
/* Main Futuristic Glass Card */
.camera-card {
  --background: #121214;
  background: radial-gradient(100% 100% at 50% 0%, rgba(168, 85, 247, 0.12) 0%, rgba(18, 18, 20, 1) 100%);
  color: #ffffff;
  border-radius: 24px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 24px 48px rgba(0, 0, 0, 0.65), 0 0 1px inset rgba(255, 255, 255, 0.2);
  margin: 16px;
  position: relative;
  overflow: hidden;
}

/* Vibrant Glow Top Edge */
.card-glow-bar {
  height: 2px;
  width: 100%;
  background: linear-gradient(90deg, transparent, #c084fc, #38bdf8, transparent);
  opacity: 0.8;
}

.camera-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 20px 12px 20px;
}

.header-left {
  display: flex;
  align-items: center;
  gap: 12px;
}

/* Pulsing Status Light */
.live-indicator {
  position: relative;
  width: 10px;
  height: 10px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.live-dot {
  width: 8px;
  height: 8px;
  background-color: #34d399;
  border-radius: 50%;
  z-index: 2;
}

.live-pulse {
  position: absolute;
  width: 16px;
  height: 16px;
  background-color: rgba(52, 211, 153, 0.4);
  border-radius: 50%;
  animation: pulse-ring 2s cubic-bezier(0.215, 0.61, 0.355, 1) infinite;
}

@keyframes pulse-ring {
  0% { transform: scale(0.5); opacity: 0.8; }
  100% { transform: scale(1.8); opacity: 0; }
}

.camera-title {
  font-size: 1.3rem;
  font-weight: 800;
  letter-spacing: -0.03em;
  color: #ffffff;
}

.camera-content {
  padding: 0 20px 20px 20px;
}

/* Interactive Viewfinder Box */
.viewfinder-deck {
  position: relative;
  height: 130px;
  background: rgba(0, 0, 0, 0.25);
  border-radius: 18px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  margin-bottom: 16px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

/* Lens Aperture Design */
.lens-container {
  position: relative;
  width: 52px;
  height: 52px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.lens-ring.outer {
  position: absolute;
  inset: 0;
  border-radius: 50%;
  border: 1px dashed rgba(192, 132, 252, 0.4);
  animation: spin 18s linear infinite;
}

.lens-ring.inner {
  position: absolute;
  inset: 5px;
  border-radius: 50%;
  border: 1px solid rgba(255, 255, 255, 0.15);
}

.lens-glass {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: radial-gradient(circle at 30% 30%, rgba(168, 85, 247, 0.4), rgba(15, 23, 42, 0.8));
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 0 12px rgba(168, 85, 247, 0.3);
}

.lens-icon {
  font-size: 1rem;
  color: #e9d5ff;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

/* Viewfinder Target Framing */
.corner {
  position: absolute;
  width: 12px;
  height: 12px;
  border-color: rgba(192, 132, 252, 0.6);
  border-style: solid;
}

.corner-tl { top: 10px; left: 10px; border-width: 2px 0 0 2px; border-top-left-radius: 4px; }
.corner-tr { top: 10px; right: 10px; border-width: 2px 2px 0 0; border-top-right-radius: 4px; }
.corner-bl { bottom: 10px; left: 10px; border-width: 0 0 2px 2px; border-bottom-left-radius: 4px; }
.corner-br { bottom: 10px; right: 10px; border-width: 0 2px 2px 0; border-bottom-right-radius: 4px; }

.deck-hint {
  margin: 0;
  font-size: 0.65rem;
  font-weight: 700;
  color: #a1a1aa;
  letter-spacing: 0.1em;
}

/* Vibrant Action Button */
.capture-button {
  --background: linear-gradient(135deg, #a855f7 0%, #7c3aed 50%, #4f46e5 100%);
  --background-hover: linear-gradient(135deg, #9333ea 0%, #6d28d9 50%, #4338ca 100%);
  --background-activated: linear-gradient(135deg, #7e22ce 0%, #5b21b6 50%, #3730a3 100%);
  --color: #ffffff;
  --border-radius: 16px;
  --box-shadow: 0 10px 24px -4px rgba(168, 85, 247, 0.5);
  height: 52px;
  margin: 0;
  transition: transform 0.2s cubic-bezier(0.2, 0.8, 0.2, 1), box-shadow 0.2s ease;
}

.capture-button:active {
  transform: scale(0.97);
}

.button-content {
  display: flex;
  align-items: center;
  gap: 8px;
}

.btn-icon {
  font-size: 1.25rem;
}

.btn-text {
  font-size: 0.95rem;
  font-weight: 700;
  letter-spacing: 0.01em;
}

/* Danger Banner */
.error-banner {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-top: 14px;
  padding: 10px 14px;
  background: rgba(239, 68, 68, 0.12);
  border: 1px solid rgba(239, 68, 68, 0.3);
  border-radius: 14px;
}

.error-chip {
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background-color: #f87171;
  color: #000000;
  font-weight: 900;
  font-size: 0.72rem;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.error-text {
  margin: 0;
  font-size: 0.82rem;
  font-weight: 500;
  color: #fca5a5;
  line-height: 1.35;
}

/* Animations */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.25s ease, transform 0.25s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(-4px);
}
</style>