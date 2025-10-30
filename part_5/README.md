# Probando aplicaciones React

## props.children y proptypes

En el siguiente ejemplo, en el componente `<LoginForm />` es un componente hijo de `<ToggableComponent>`.

Es posible agregar cualquier elemento de React entre las etiquetas de apertura y cierre de un componente padre.

```js
import ToggleButton from './components/ToggleButtonmport'
import LoginForm from './components/LoginForm'

const App = () => {
    return(
        <ToggleButton>
            <LoginForm /> {/* Componente hijo */}
        </ToggleButton>
    )
}

// Componente ToggableButton.jsx
const ToggableButton = (props) => {
    return(
        <div>
            { props.children }
            <button onClick={() => {toggleVisibility}}>cancel</button>
        </div>
    )
}
```

### props.children

```js
<div>
    {/* props.children se utiliza para hacer referencia a los componentes hijos de un componente // en este caso al componente <LoginForm> como se definio en el ejemplo anterior */}
    { props.children }
    <button onClick={() => {toggleVisibility}}>cancel</button>
</div>
```

A diferencia de las `props` normales, React agrega automaticamente `children` y siempre existe. Si un componente padre se define con una etiqueta de cierre automatico `</>` (de esta manera no hay forma de que contenga ningun componente hijo), `props.children` por defecto sera un array vacio.

## Estado de los formularios

A veces se requiere que el estado de los componentes cambie siempre al mismo tiempo, para hacerlo se debe eliminar el estado utilizado en 2 o mas componentes y moverlo al componente padre mas cercano que tengan en comun, lo cual se conoce como elevar el estado.

## Referencias a componentes con ref

Permite el acceso a las funciones de un componente desde fuera del mismo.

```js
import {useState, useEffect, useRef} from 'react'

const App = () => {
    const noteFormRef = useRef()

    const noteForm = () => {
        <Togglable buttonLabel="new note" ref={noteFormRef} >
            <NoteForm  createNote={addNote} />
        </Togglable>
    }
}
```

El hook `useRef` se utiliza para crear una referencia llamada `noteFormRef` que se asigna al componente `<Togglable>` que contiene a su vez otro componente como hijo. Este hook asegura que se mantenga la misma referencia (`ref`) en todas las renderizaciones del componente.

```js
import {useState, forwardRef, useImperativeHandle} from 'react'

// La funcion que crea el componente esta envuelta dentro de una llamada a la funcion `forwardRef`, asi el componente puede acceder a la referencia que le fue asignada 
const Togglable = forwardRef((props, refs) => {
    const [visibility, setVisibility] = useState()

    const toggleVisibility = () => {
        setVisibility(!visibility)        
    }

    useImperativeHandle(refs, () => {
        return {
            toggleVisibility
        }
    })
})
```

`useImperativeHandle` hace que que la funcion `toggleVisibility()` este disponible fuera del componente `Togglable`.

```js
const App = () => {
    const addNote = (noteObject) => {
        noteFormRef.current.toggleVisibility()
    }
}
```

> La funcion `useImperativeHandle` es un hook de React que se usa para definir funciones en un componente que se pueden invocar desde fuera del mismo.
