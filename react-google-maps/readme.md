````javascript
import { useState } from 'react'
import { GoogleMap, useJsApiLoader, Marker } from '@react-google-maps/api';

function CustomMap() {

  const { isLoaded } = useJsApiLoader({
    googleMapsApiKey: "_____TU_API_KEY_____"
  })

  const [map, setMap] = useState(null)

  const onLoad = (map) => console.log('Aquí haz lo que necesites tras la carga del mapa')
  const onUnmount = () => setMap(null)

  return isLoaded && (
    <GoogleMap
      mapContainerStyle={{ height: '800px' }}
      zoom={12}
      onLoad={onLoad}
      center={{ lat: -3.745, lng: -38.523 }}
      onUnmount={onUnmount}

    >
      <Marker position={{ lat: -3.745, lng: -38.523 }} />
      <Marker position={{ lat: -3.725, lng: -38.513 }} />
      <Marker position={{ lat: -3.735, lng: -38.533 }} />

    </GoogleMap>
  )
}

export default CustomMap
````
