<script setup>
import { ref, onMounted } from 'vue'

const showCamera = ref(false)
const videoElement = ref(null)
const canvasElement = ref(null)
const capturedImage = ref(null)

async function openCamera() {
  try {
    if (videoElement.value) {
      const stream = await navigator.mediaDevices.getUserMedia({
        video: {
          facingMode: 'environment',
        },
      })
      videoElement.value.srcObject = stream
      showCamera.value = true
    }
  } catch (error) {
    console.error('Kamera hiba:', error)
    alert('Nem sikerült elérni a kamerát')
  }
}

function capturePhoto() {
  const canvas = canvasElement.value
  const video = videoElement.value

  if (canvas && video) {
    canvas.width = video.videoWidth
    canvas.height = video.videoHeight

    canvas.getContext('2d').drawImage(video, 0, 0)
    capturedImage.value = canvas.toDataURL('image/jpeg')
  }
}
</script>
<template>
  <section>
    <div class="prayer-con">
      <h1 class="prayer-con__h1 text-color-w">
        Ne hagyd, hogy az idő múlása megakadályozzon abban, hogy közelebb kerülj
        a Teremtőhöz
      </h1>

      <div class="koshersee">
        <button class="text-color-w" @click="openCamera">
          Kamera megnyitása
        </button>
        <video v-show="showCamera" ref="videoElement" autoplay></video>
        <button class="text-color-w" v-if="showCamera" @click="capturePhoto">
          Fénykép készítése
        </button>
        <canvas ref="canvasElement" style="display: none"></canvas>
        <NuxtImg class="koshersee__img"
          v-if="capturedImage"
          :src="capturedImage"
          alt="Készített fénykép"
        />
      </div>
      <div class="box__content">
        <div class="box__item">
          <div class="box__item-icon">
            <NuxtImg
              src="/img/body/shacharit.svg"
              alt="Magyar Hidabroot"
              class="box__item-icon__img"
              height="100%"
            />
          </div>
          <div class="box__item-label text-color-w">Shacharit</div>
        </div>

        <div class="box__item">
          <div class="box__item-icon">
            <NuxtImg
              src="/img/body/mincha.svg"
              alt="Magyar Hidabroot"
              class="box__item-icon__img"
              height="100%"
            />
          </div>
          <div class="box__item-label text-color-w">Mincha</div>
        </div>

        <div class="box__item">
          <div class="box__item-icon">
            <NuxtImg
              src="/img/body/arvit.svg"
              alt="Magyar Hidabroot"
              class="box__item-icon__img"
              height="100%"
            />
          </div>
          <div class="box__item-label text-color-w">Arvit</div>
        </div>
      </div>
      <p class="box__item__p text-color-w">
        Töltsd le az appot
        <strong class="box__item-label__st"><i>Te is</i></strong
        >, hogy mindig a megfelelő időben kapj értesítést. Az emlékeztetők
        biztosítják, hogy egyetlen ima se maradjon ki – hiszen minden ima egy
        lépés a lélek felemelése felé.
      </p>
      <div class="subscription-form">
        <button class="subscription-form__button" type="submit">
          Én is letöltöm
        </button>
        <p class="box__item__p text-color-w">
          Legyél része egy közösségnek, amely mindig emlékeztet az igazán fontos
          dolgokra.
        </p>
      </div>
    </div>
  </section>
</template>
