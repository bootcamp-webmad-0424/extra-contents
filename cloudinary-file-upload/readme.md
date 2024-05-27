# Subida de archivos asíncrona con Cloudinary

## Pasos previos
Pasos previos a la integración del sistema de subida asíncrona:
- Instalar en el servidor de Express las dependencias `cloudinary@1`, `multer` y `multer-storage-cloudinary` - ATENCIÓN al instalar cloudinary, debe ser en su versión 1: `npm i cloudinary@1`
- Registrarse en [Cloudinary](https://www.cloudinary.com).
- Crear en el servidor las variables de entorno `CLOUDINARY_NAME`, `CLOUDINARY_KEY` y `CLOUDINARY_SECRET` con los valores disponibles en el Dashboard tras registrar tu cuenta de Cloudinary:<img width="780" alt="image" src="https://github.com/bootcamp-webmad-0124/extra-contents/assets/46670724/0d4900b2-571a-4728-94d0-8ae9f7fe2e35">


## Grabación de la integración
Disponible [en este enlace](https://drive.google.com/file/d/1pnRLK_7eLuUoMJnjeHw0Pl2RlCGZ2-nl/preview)

## Códigos de la integración 

### Multer middleware (server)
Ruta `/middleware/uploader.middleware.js`

````javascript
const cloudinary = require('cloudinary').v2
const { CloudinaryStorage } = require('multer-storage-cloudinary')
const multer = require('multer')
 
cloudinary.config({
    cloud_name: process.env.CLOUDINARY_NAME,
    api_key: process.env.CLOUDINARY_KEY,
    api_secret: process.env.CLOUDINARY_SECRET
})
 
const storage = new CloudinaryStorage({ cloudinary })
 
module.exports = multer({ storage })
````

### File upload route (server)
Ruta `/routes/upload.router.js`

````javascript
const router = require("express").Router()
 
const uploader = require('./../middleware/cloudinary.middleware')
 
router.post('/image', uploader.single('imageData'), (req, res) => {
 
  if (!req.file) {
    res.status(500).json({ errorMessage: 'Error caragndo el archivo' })
    return
  }
 
  res.json({ cloudinary_url: req.file.path })
})
 
 
module.exports = router
````

### File upload service (client)
Ruta `/services/upload.services.js`

````javascript
import axios from 'axios'
 
class UploadServices {
 
    constructor() {
 
        this.api = axios.create({
            baseURL: `${process.env.REACT_APP_API_URL}/upload`
        })
    }
 
    uploadimage(imageForm) {
        return this.api.post('/image', imageForm)
    }
}
 
const uploadServices = new UploadServices()
 
export default uploadServices
````

### File upload field (client)
Ruta `/components/NewThingForm/NewThingForm.jsx`

````javascript
<Form.Group className="mb-3" controlId="image">
	<Form.Label>Imagen (URL)</Form.Label>
	<Form.Control type="file" onChange={handleFileUpload} />
</Form.Group>
````

### File upload handler (client)
Ruta `/components/NewThingForm/NewThingForm.jsx`

````javascript
const handleFileUpload = e => {
 
	const formData = new FormData()
	formData.append('imageData', e.target.files[0])
 
	uploadServices
		.uploadimage(formData)
		.then(res => {
			setCoasterData({ ...coasterData, imageUrl: res.data.cloudinary_url })
		})
		.catch(err => console.log(err))
}
````
