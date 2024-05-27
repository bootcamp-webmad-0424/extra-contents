# Datos geoespaciales en MongoDB 

## Modelo

GeoJSON se utiliza en MongoDB para almacenar datos que representen información geoespacial en una esfera similar a la Tierra. GeoJSON se compone de una o varias posiciones y un tipo:

````javascript
// Ejemplo de modelo para restaurante con datos geoespaciales

// models/Restaurant.model.js

const { Schema, model } = require('mongoose')

const restaurantSchema = new Schema({
  name: {
    type: String
  },
  description: {
    type: String
  },
  location: {                                           // añadir
    type: {
      type: String
    },
    coordinates: {
      type: [Number]
    }
  }

})

restaurantSchema.index({ location: '2dsphere' })        // añadir

module.exports = model('Restaurant', restaurantSchema)
````


## Ejemplo de petición

<img width="1000" alt="image" src="https://github.com/bootcamp-webmad-0124/extra-contents/assets/46670724/3ed98b0f-fcae-4982-9681-758c3e51796e">


## Ejemplo de endpoint

````javascript
// Ejemplo de ruta para restaurante con datos geoespaciales

// routes/restaurant.routes.js

router.post("/", (req, res, next) => {

  const { name, description, latitude, longitude } = req.body

  const location = {
    type: 'Point',
    coordinates: [longitude, latitude]                // primero longitud, luego latitud
  }

  Restaurant
    .create({ name, description, location })
    .then(() => res.sensatatus(200))
    .catch(err => next(err))
})
````



## Resultado en BBDD

<img width="965" alt="image" src="https://github.com/bootcamp-webmad-0124/extra-contents/assets/46670724/6295d8f9-c069-4902-8896-620bbc7f3e6e">


