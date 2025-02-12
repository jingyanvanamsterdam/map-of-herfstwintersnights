<template>
  <div id="mapComp" class="w-screen h-screen">
  </div>
</template>

<script setup>
import L, { map } from 'leaflet'
import { onMounted, ref } from 'vue'
import snow from '../assets/icons/snow.png'
import moon from '../assets/icons/moon.png'
import spiral from '../assets/icons/spiral.png'
import eye from '../assets/icons/eye.png'
import RosjPina from '../assets/pic/RosjPina.jpeg'
import Asmtelpark from '../assets/pic/Amstelpark.jpeg'
import AmsterdamBos from '../assets/pic/AmsterdamBos.jpeg'
import Brug831 from '../assets/pic/Brug831.jpeg'
import Amstelpark01 from '../assets/sounds/Amstelpark01.mp3'
import Amstelpark02 from '../assets/sounds/Amstelpark02.mp3'
import Amstelpark03 from '../assets/sounds/Amstelpark03.mp3'
import Amstelpark04 from '../assets/sounds/Amstelpark04.mp3'
import Amstelpark05 from '../assets/sounds/Amstelpark05.mp3'
import Amstelveen01 from '../assets/sounds/Amstelveen01.mp3'
import Amstelveen02 from '../assets/sounds/Amstelveen01.mp3'
import Amstelveen03 from '../assets/sounds/Amstelveen01.mp3'
import Amstelveen04 from '../assets/sounds/Amstelveen01.mp3'
import AmsterdamBos01 from '../assets/sounds/AmsterdamBos01.mp3'
import AmsterdamBos02 from '../assets/sounds/AmsterdamBos02.mp3'
import JacobvanLennepkade from '../assets/sounds/JacobvanLennepkade.mp3'
import NoordPark01 from '../assets/sounds/NoordPark01.mp3'
import NoordPark02 from '../assets/sounds/NoordPark02.mp3'
import Rembrandtpark01 from '../assets/sounds/Rembrandtpark01.mp3'
import Rembrandtpark02 from '../assets/sounds/Rembrandtpark02.mp3'
import Rembrandtpark03 from '../assets/sounds/Rembrandtpark03.mp3'
import Rembrandtpark04 from '../assets/sounds/Rembrandtpark04.mp3'
import Rembrandtpark05 from '../assets/sounds/Rembrandtpark05.mp3'
import Rembrandtpark06 from '../assets/sounds/Rembrandtpark06.mp3'
import Rembrandtpark07 from '../assets/sounds/Rembrandtpark07.mp3'
import SportcentrumAmstelpark from '../assets/sounds/SportcentrumAmstelpark.mp3'
import AmsterdamZuidPark from '../assets/sounds/AmsterdamZuidPark.mp3'
const parkMarkersVisible = ref(false)

onMounted(()=>{
  const tileLayer = L.tileLayer('https://tiles.stadiamaps.com/tiles/stamen_toner/{z}/{x}/{y}{r}.{ext}', {
    minZoom: 13,
    maxZoom: 20,
    ext: 'png'
  })

  const mapComp = L.map('mapComp', {
    center: [52.350436, 4.903430], 
    zoom: 13, 
    minZoom: 13, 
    maxZoom: 19, 
    zoomDelta: 2, // zoom in and out 2 levels every click
    layers: [tileLayer] //arrays of layers that will be added to the map initially
  })

  const createAudioMarker = (coords, audioSrc) => {
    return L.marker(coords, {icon: spiralIcon}).bindPopup(`
      <audio controls>
        <source src="${audioSrc}" type="audio/mpeg">
      </audio>
    `)
  }

  const createPictureMarker = (coords, imgSrc) => {
    return L.marker(coords, {icon: moonIcon}).bindPopup(`
      <img src="${imgSrc}" style="border-radius: 3%"/>
    `, {
      className: 'pictures'
    })
  }

  /*  ----------------      Create Icons   -------------------   */
  const snowIcon = L.icon({
    iconUrl: snow,
    iconSize: [50, 50], 
    iconAnchor: [25, 25], 
    className: 'icons'
  })
  const moonIcon = L.icon({
    iconUrl: moon,
    iconSize: [70, 70], 
    iconAnchor: [35, 35], 
    className: 'icons'
  })
  const spiralIcon = L.icon({
    iconUrl: spiral,
    iconSize: [50, 50], 
    iconAnchor: [25, 25], 
    className: 'icons'
  })
  const eyeIcon = L.icon({
    iconUrl: eye,
    iconSize: [70, 70], 
    iconAnchor: [35, 35],  
    className: 'icons'
  })

  const createGlowingSpot = (coords, iconSize, iconAnchor) => {
    const shiningSpotIcon = L.divIcon({
      className: 'shining-icon', // The class of the icon container 
      html: `<div class="shining-spot" style="width: ${iconSize[0]}px; height: ${iconSize[0]}px; background: radial-gradient(circle, rgba(255,223,0,1) 17%, rgba(255,223,0,0.5) 54%); border-radius: 50%; animation: pulsate 1.5s infinite alternate"></div>`, // class of the shining icon
      iconSize: iconSize,
      iconAnchor: iconAnchor
    })
    return L.marker(coords, { icon: shiningSpotIcon})
  }

  const oosterparkspot = createGlowingSpot([52.359925, 4.919944], [18, 18], [9, 9])
  const rosjspot = createGlowingSpot([52.326985, 4.886768], [18, 18], [9, 9])
  const brug831spot = createGlowingSpot([52.329436, 4.875567], [18, 18], [9, 9])
  const jacobspot = createGlowingSpot([52.364036, 4.865890], [18, 18], [9, 9])
  const noordspot = createGlowingSpot([52.390205, 4.918064], [26, 26], [13, 13])
  const amstelparkspot = createGlowingSpot([52.329807, 4.894604], [56, 56], [28, 28])
  const rembrandtspot = createGlowingSpot([52.363414, 4.847365], [56, 56], [28, 28])
  const amstelveenspot = createGlowingSpot([52.322482, 4.872916], [38, 38], [19, 19])
  const amsterdambosspot = createGlowingSpot([52.323162, 4.843944], [30, 30], [15, 15])
  

  /*  ----------------      Pictures   -------------------   */
  const rosjpina = createPictureMarker([52.326985, 4.886768], RosjPina)
  const amstelpark = createPictureMarker([52.331598, 4.892433], Asmtelpark)
  const brug831 = createPictureMarker([52.329436, 4.875567], Brug831)
  const amsterdambos = createPictureMarker([52.324957, 4.849677], AmsterdamBos)

  /*  ----------------      Audios   -------------------   */
  const amstelpark1 = createAudioMarker([52.328709, 4.892334], Amstelpark01)
  const amstelpark2 = createAudioMarker([52.330989, 4.896202], Amstelpark02)
  const amstelpark3 = createAudioMarker([52.328331, 4.895569], Amstelpark03)
  const amstelpark4 = createAudioMarker([52.329807, 4.894604], Amstelpark04)
  const amstelpark5 = createAudioMarker([52.329884, 4.892458], Amstelpark05)
  const sportcentrumAmstelpark = createAudioMarker([52.333088, 4.893924], SportcentrumAmstelpark)
  
  const amstelveen1 = createAudioMarker([52.322482, 4.872916], Amstelveen01)
  const amstelveen2 = createAudioMarker([52.322479, 4.878012], Amstelveen02)
  const amstelveen3 = createAudioMarker([52.322593, 4.874103], Amstelveen03)
  const amstelveen4 = createAudioMarker([52.322141, 4.876142], Amstelveen04)

  const amsterdmbos1 = createAudioMarker([52.319007, 4.837309], AmsterdamBos01)
  const amsterdmbos2 = createAudioMarker([52.325071, 4.838875], AmsterdamBos02)

  const jacobvanLennepkade = createAudioMarker([52.364036, 4.865890], JacobvanLennepkade)
  const amsterdamZuidPark = createAudioMarker([52.359925, 4.919944], AmsterdamZuidPark)

  const noordpark1 = createAudioMarker([52.390205, 4.918064], NoordPark01)
  const noordpark2 = createAudioMarker([52.390716, 4.917189], NoordPark02)

  const rembrandtpark1 = createAudioMarker([52.363414, 4.847365], Rembrandtpark01)
  const rembrandtpark2 = createAudioMarker([52.361961, 4.847527], Rembrandtpark02)
  const rembrandtpark3 = createAudioMarker([52.358650, 4.844910], Rembrandtpark03)
  const rembrandtpark4 = createAudioMarker([52.365492, 4.846587], Rembrandtpark04)
  const rembrandtpark5 = createAudioMarker([52.366965, 4.848028], Rembrandtpark05)
  const rembrandtpark6 = createAudioMarker([52.366688, 4.844033], Rembrandtpark06)
  const rembrandtpark7 = createAudioMarker([52.360334, 4.847652], Rembrandtpark07)


  /* ----------------      Poems   -------------------
  const flevoPoem1 = L.marker([52.359098, 4.951112], {
    icon: eyeIcon
  }).bindTooltip('Writing in an unfamiliar musical form is not something peculiar that no one has not done before.')
  */

  /*  ----------------      LayerGroup   -------------------   */
  const soundLayer = L.layerGroup([amstelpark1, amstelpark2, amstelpark3, amstelpark4,amstelpark5, sportcentrumAmstelpark, amstelveen1, amstelveen2, amstelveen3, amstelveen4, amsterdmbos1, amsterdmbos2, jacobvanLennepkade, amsterdamZuidPark, noordpark1, noordpark2, rembrandtpark1, rembrandtpark2, rembrandtpark3, rembrandtpark4, rembrandtpark5, rembrandtpark6, rembrandtpark7])

  const picLayer = L.layerGroup([rosjpina, amstelpark, brug831, amsterdambos])

  //const poemLayer = L.layerGroup([flevoPoem1, flevoPoem2, flevoPoem3])

  const spotsLayer = L.layerGroup([oosterparkspot, rosjspot, brug831spot, jacobspot, noordspot, rembrandtspot, amstelparkspot, amstelveenspot, amsterdambosspot])

  /*  ----------------      Overlay Maps setting   -------------------   */
  const overlayMaps = {
    //"Poem: should change to an abstract name": poemLayer, 
    "Paintings": picLayer, 
    "Audios": soundLayer, 
  }
  const baseLayer =  {
    "A Reverie in Herfstwinter's Nights": tileLayer
  }

  const layerControl = L.control.layers(baseLayer, overlayMaps).addTo(mapComp)

  const updateMarkerVisibility = () => {
    const currentZoom = mapComp.getZoom()
    if(currentZoom >=16){
      //poemLayer.addTo(mapComp)
      soundLayer.addTo(mapComp)
      picLayer.addTo(mapComp)
      mapComp.removeLayer(spotsLayer)
      parkMarkersVisible.value = true
    } else {
      if (parkMarkersVisible.value) {
        //mapComp.removeLayer(poemLayer)
        mapComp.removeLayer(soundLayer)
        mapComp.removeLayer(picLayer)
        parkMarkersVisible.value = false
      } 
      spotsLayer.addTo(mapComp)
    }
  }
  // Add zoom event listener -- Map state change events， zoomend fire updateMarkerVisiblity
  mapComp.on('zoom', updateMarkerVisibility)
  
  // Initial check for marker visibility
  updateMarkerVisibility();


  //Foot Steps
  //L.geoJSON(footSteps).addTo(mapComp)
  /*
  const timestamps = footSteps.features[0].properties.timestamps;
  const coordinates = footSteps.features[0].geometry.coordinates;
  let footMarker = L.marker(coordinates[0].reverse(), {icon: snowIcon})
  let step = 0
  let previousFoot = null
  const moveFootStep = () => {
    if(step < coordinates.length - 2){
      setTimeout(()=>{
        if(previousFoot){
          mapComp.removeLayer(previousFoot)
        }
        //add new step
        previousFoot = L.marker(coordinates[step].reverse(), {icon: snowIcon}).addTo(mapComp)
        //move the marker
        footMarker.setLatLng(coordinates[step+1].reverse())
        step++
        moveFootStep() //递归调用
      }, timestamps[step+1] - timestamps[step])
    }
  }
  footMarker.on('click', moveFootStep)
  */
})





</script>

<style>
  @keyframes pulsate {
  0% {
    box-shadow: 0 0 10px rgba(255, 223, 0, 1);
    transform: scale(1)
  }
  60% {
    box-shadow: 0 0 40px rgba(255, 223, 0, 1);
    transform: scale(1.5) 
  }
  100% {
    box-shadow: 0 0 10px rgba(255, 223, 0, 1);
    transform: scale(1) 
  }
}
</style>
