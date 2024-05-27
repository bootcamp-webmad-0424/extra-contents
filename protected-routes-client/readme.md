# Rutas protegidas con React Router V6

## Pasos previos
Pasos previos a la integración de la protección de rutas
- Disponer de todo el sistema de Auth creado y testeado tanto en cliente (incluyendo `auth.context.jsx`) como en servidor.

## Grabación de la integración
Disponible [en este enlace](https://drive.google.com/file/d/1AtOWcoICae4Da9HgOgaFvbRKfycibfYF/preview)

## Códigos de la integración 

### Ruta protegida (client)
Ruta `/routes/PrivateRoute.jsx`

````javascript
import { useContext } from "react"
import { Outlet, Navigate } from "react-router-dom"
import Loader from "../components/Loader/Loader"
import { AuthContext } from "../contexts/auth.context"
 
const PrivateRoute = () => {
 
    const { user, isLoading } = useContext(AuthContext)
 
    if (isLoading) {
        return <Loader />
    }
 
    if (!user) {
        return <Navigate to="/inicio-sesion" />
    }
 
    return <Outlet />
}
 
export default PrivateRoute
````

### Estado de carga en contexto de Auth (cliente)
Ruta `/contexts/auth.context.jsx`

````javascript
import React, { useState, useEffect } from "react"
import axios from 'axios'
const API_URL = 'http://localhost:5005'

const AuthContext = React.createContext()

function AuthProviderWrapper(props) {

    useEffect(() => authenticateUser(), [])

    const [user, setUser] = useState(null)
    const [isLoggedIn, setIsLoggedIn] = useState(false)
    const [isLoading, setIsLoading] = useState(true)    // AÑADIR

    const storeToken = tokenValue => {
        localStorage.setItem('authToken', tokenValue)
    }

    const authenticateUser = () => {

        const storedToken = localStorage.getItem('authToken')

        if (storedToken) {

            axios
                .get(`${API_URL}/api/auth/verify`, {
                    headers: { Authorization: `Bearer ${storedToken}` }
                })
                .then((response) => {
                    const { userInfo } = response.data;
                    setIsLoggedIn(true)
                    setUser(userInfo)
                    setIsLoading(false)        // AÑADIR
                })
                .catch((error) => {
                    logout()
                })
        } else {
            logout()
        }
    }

    const logout = () => {
        setUser(null)
        setIsLoggedIn(false)
        localStorage.removeItem('authToken')
        setIsLoading(false)                    // AÑADIR
    }

    return (
        <AuthContext.Provider value={{ user, isLoggedIn, storeToken, authenticateUser, logout }}>
            {props.children}
        </AuthContext.Provider>
    )
}

export { AuthProviderWrapper, AuthContext }
````


