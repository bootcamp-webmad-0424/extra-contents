# Obtener direcciones y posiciones de lugares

La dependencia [React Google Places Autocomplete](https://www.npmjs.com/package/react-google-places-autocomplete) permite convertir un texto en un objeto con su posición en términos de latitud y longitud.

## Pasos previos
Pasos previos a la integración de la dependencia:
- Instalar en cliente la dependencia `npm i react-google-places-autocomplete`
- Disponer de un componente sobre el que incluir el campo de texto para escribir la dirección

## Grabación de la integración
Disponible [en este enlace](https://drive.google.com/file/d/1mPIffVZD0exq7V89AN0RUrMlg0d-atI2/preview)

## Integración
La dependencia ofrece dos métodos:
- `geocodeByAddress` permite obtener los detalles de un lugar a raíz de un string con una dirección
- `getLatLng` permite obtener la latitud y longitud de un lugar a raíz de uno de los objetos retornados por `geocodeByAddress`

Importar en el componente de formulario los métodos:
````javascript
import { geocodeByAddress, getLatLng } from 'react-google-places-autocomplete'
````

Crear un estado para almacenar la dirección
````javascript
const [addressValue, setAddressValue] = useState()
````

Incluir en el formulario el componente, que va a renderizar un campo de dirección:
````javascript
<GooglePlacesAutocomplete
  selectProps={{
    addressValue,
    onChange: setAddressValue,
  }}
  apiKey="____TU_API_KEY____"
/>
````

Detectar un cambio sobre la variable de estado:
````javascript
useEffect(() => handleAutocomplete(), [addressValue])
````

Obtener las coordenadas a través de los métodos de la dependencia:
````javascript
const handleAutocomplete = () => {

  addressValue.label && geocodeByAddress(addressValue?.label)
    .then(([addressDetails]) => getLatLng(addressDetails))
    .then((coordinates) => {
      console.log('LAS COORDENADAS', coordinates)
    })
    .catch(error => console.error(error))
}
````
