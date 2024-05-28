# Interceptores de Axios para cabecera de auth

Grabación de la explicación [en este enlace](https://drive.google.com/drive/folders/1oBmkaZbY6VZH_yOZyXaIVmZtR5U4OZYk?usp=drive_link)

---

Axios permite incorporar interceptores a una aplicación de Axios, performando la operación deseada previo a la emisión de cualquier request.

Ejemplo de interceptores para incluir token de autorización en la cabecera:

````javascript
this.axiosApp.interceptors.request.use(config => {

    const storedToken = localStorage.getItem('authToken');

    if (storedToken) {
        config.headers = { Authorization: `Bearer ${storedToken}` };
    }

    return config
})
````
