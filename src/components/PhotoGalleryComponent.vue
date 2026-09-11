<template>
  <ion-card class="main-card">
    <ion-card-header>
      <div class="header-content">
        <div class="header-left">
          <ion-card-title class="gallery-title">Photo Gallery</ion-card-title>
          <div class="gallery-subtitle-tag">
            <span class="header-tag">✨ Memories</span>
          </div>
        </div>
        <span v-if="photos.length > 0" class="photo-badge">
          {{ photos.length }} {{ photos.length === 1 ? 'photo' : 'photos' }}
        </span>
      </div>
    </ion-card-header>

    <ion-card-content class="card-content-wrapper">
      <!-- Empty State -->
      <div v-if="photos.length === 0" class="empty-gallery">
        <div class="empty-icon-circle">
          <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round">
            <path d="M14.5 4h-5L7 7H4a2 2 0 0 0-2 2v9a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2V9a2 2 0 0 0-2-2h-3l-2.5-3z"/>
            <circle cx="12" cy="13" r="3"/>
          </svg>
        </div>
        <p class="empty-title">No pictures yet.</p>
        <p class="empty-subtitle">Take a picture using the camera.</p>
      </div>

      <!-- Responsive Photo Grid -->
      <ion-grid v-else class="ion-no-padding">
        <ion-row class="gallery-row">
          <ion-col
            v-for="(photo, index) in photos"
            :key="index"
            size="6"
            size-sm="4"
            size-md="3"
            size-lg="2.4"
            class="gallery-col"
          >
            <ion-card class="photo-card">
              <div class="image-wrapper">
                <ion-img :src="photo" class="gallery-image" />
                <div class="image-overlay" />
              </div>

              <!-- Unique Tag persistent to each specific picture -->
              <div class="photo-footer">
                <span class="category-tag">
                  {{ getPhotoTag(photo) }}
                </span>
              </div>
            </ion-card>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-card-content>
  </ion-card>
</template>

<script setup lang="ts">
import {
  IonCard,
  IonCardContent,
  IonCardHeader,
  IonCardTitle,
  IonGrid,
  IonRow,
  IonCol,
  IonImg,
} from "@ionic/vue";

defineProps<{ photos: string[] }>();

// Collection of unique captions
const uniqueCaptions = [
  "✨ Captured Moment",
  "📸 Frozen in Time",
  "⚡ Golden Hour",
  "✨ Lifetime Memory",
  "💎 Pure Nostalgia",
  "🌌 Snapshot",
  "☀️ Bright Days",
  "🌿 Good Vibes",
  "🎉 Special Day",
  "🔮 Unforgettable",
  "💫 Pure Magic",
  "🎞️ Timeless Frame"
];

// Generates a consistent hash from the image string so each picture retains its own specific tag permanently
const getPhotoTag = (photoSrc: string): string => {
  if (!photoSrc) return uniqueCaptions[0];
  let hash = 0;
  for (let i = 0; i < photoSrc.length; i++) {
    hash = (hash << 5) - hash + photoSrc.charCodeAt(i);
    hash |= 0;
  }
  const positiveIndex = Math.abs(hash) % uniqueCaptions.length;
  return uniqueCaptions[positiveIndex];
};
</script>

<style scoped>
/* Main Dark Container */
.main-card {
  --background: #121214;
  background: #121214;
  color: #ffffff;
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow: 0 16px 32px rgba(0, 0, 0, 0.5);
  margin: 16px;
}

.header-content {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
}

.header-left {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.gallery-title {
  font-size: 1.4rem;
  font-weight: 700;
  letter-spacing: -0.02em;
  color: #ffffff;
}

/* "Memories" Tag under Header Title */
.header-tag {
  display: inline-flex;
  align-items: center;
  background: rgba(168, 85, 247, 0.15);
  color: #c084fc;
  font-size: 0.72rem;
  font-weight: 600;
  padding: 3px 10px;
  border-radius: 12px;
  border: 1px solid rgba(168, 85, 247, 0.3);
  letter-spacing: 0.02em;
}

.photo-badge {
  font-size: 0.8rem;
  font-weight: 600;
  color: #a1a1aa;
  background: rgba(255, 255, 255, 0.08);
  padding: 4px 10px;
  border-radius: 12px;
}

.card-content-wrapper {
  padding-top: 8px;
}

/* Responsive Grid Spacing */
.gallery-row {
  margin: -6px;
}

.gallery-col {
  padding: 6px;
}

/* Individual Item Card */
.photo-card {
  --background: #1c1c21;
  background: #1c1c21;
  margin: 0;
  border-radius: 14px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
  border: 1px solid rgba(255, 255, 255, 0.05);
  transition: transform 0.25s cubic-bezier(0.2, 0.8, 0.2, 1),
              box-shadow 0.25s cubic-bezier(0.2, 0.8, 0.2, 1);
}

@media (hover: hover) {
  .photo-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.6);
  }

  .photo-card:hover .gallery-image::part(image) {
    transform: scale(1.06);
  }
}

.image-wrapper {
  position: relative;
  width: 100%;
  aspect-ratio: 1 / 1;
  overflow: hidden;
  background-color: #27272a;
}

.gallery-image {
  width: 100%;
  height: 100%;
  display: block;
}

.gallery-image::part(image) {
  object-fit: cover;
  transition: transform 0.35s cubic-bezier(0.2, 0.8, 0.2, 1);
}

.image-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(0, 0, 0, 0) 60%, rgba(0, 0, 0, 0.35) 100%);
  pointer-events: none;
}

/* Tag Container Underneath Image */
.photo-footer {
  padding: 10px 12px;
  background: #1c1c21;
  display: flex;
  align-items: center;
}

/* Catchy Glowing Tag Pill Style */
.category-tag {
  display: inline-flex;
  align-items: center;
  gap: 4px;
  background: rgba(168, 85, 247, 0.15);
  color: #c084fc;
  font-size: 0.72rem;
  font-weight: 600;
  letter-spacing: 0.02em;
  padding: 4px 10px;
  border-radius: 12px;
  border: 1px solid rgba(168, 85, 247, 0.3);
}

/* Empty State Styling */
.empty-gallery {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 48px 16px;
  text-align: center;
  background: rgba(255, 255, 255, 0.02);
  border: 1px dashed rgba(255, 255, 255, 0.12);
  border-radius: 16px;
}

.empty-icon-circle {
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.05);
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 12px;
  color: #a1a1aa;
}

.empty-title {
  margin: 0 0 4px 0;
  font-size: 1rem;
  font-weight: 600;
  color: #ffffff;
}

.empty-subtitle {
  margin: 0;
  font-size: 0.85rem;
  color: #71717a;
}
</style>