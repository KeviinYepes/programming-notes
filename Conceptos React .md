# ¿Qué es React?
React es una biblioteca de JavaScript de código abierto diseñada para crear interfaces de usuario.

# Ventajas de React

1. Fácil de aprender.  
2. Componentes reutilizables.  
3. Crear aplicaciones dinámicas.  
4. Buen rendimiento.  

# Conceptos básicos de React

## Componentes
Es una parte de la interfaz de usuario, independiente y reutilizable.

### Componente funcional  

Son funciones de JavaScript que retornan un elemento de React (JSX).  
*Ejemplo*  
````bash
function Greeting(props) {
  return <h1>¡Hola, {props.name}!</h1>;
}
````
### Componente de Clase
Clase de ES6 (JavaScript Moderno) que retorna un elemento JSX.
*Ejemplo*
````bash
class saludo extends React.Component {
  render() {
        return <h1>¡Hola, {this.props.nombre}!</h1>
    }
}
````

## Props
En React, las props (abreviatura de "properties" o propiedades) son un mecanismo para pasar datos de un componente padre a un componente hijo, permitiendo la comunicación y la creación de componentes reutilizables.

### Características principales de las props:
1. Comunicación unidireccional son datos fluyen siempre de un componente padre a sus componentes hijos, nunca al revés.
2. Inmutabilidad son los props de solo lectura; el componente hijo no puede cambiar los valores que recibe del padre.

## Estado
SetState() programa una actualización al objeto estado de un componente. Cuando el estado cambia, el componente responde volviendo a renderizar

## Hook
Son funciones especiales que permiten a los componentes funcionales acceder a características de React que antes solo estaban disponibles en los componentes de clase, como el estado (state) y el ciclo de vida (lifecycle), sin necesidad de escribir una clase

## Event Listener
Función ejecutada cunado ocurre un evento específico.

