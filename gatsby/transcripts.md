# Gatsby tutorials transcripts

## que forman una pagina gatsby

- explicar el comando gatsby new
- explicar las paginas de gatsby
- gatsby develop y ver como cambiando algo se actualiza automaticamente
- agregar estilos a la pagina
- crear about.js
- crear Header.js
- agregar headerText prop
- Link entre paginas

## Intro Gatsby

Lo primero que tenemos que hacer para crear nuestra primera web con Gatsby, es ejecutar el comando `npx gatsby new hola-mundo`
Una vez esto ha terminado, podemos navegar a la carpeta nueva y ejecutar el comando `npm run develop`, esto ejecutara internamente el comando `gatsby develop`, que es el que levantara nuestra web. Como puedes ver. Cuando esto termine, podremos acceder a nuestra web en la direccion localhost:8000

## Estilos en Gatsby

[Tutorial Gatsbyjs - Estilos (CSS) - CodeSandbox](https://hhg.link/gatsby-estilos-codigo)
Toda pagina web cargan de alguna u otra manera estilos globales, estilos que afectan de manera general a todos los elementos; como la tipografía, colores de fondo, etc. Una de las maneras mas fáciles de agregar estilos globales en un proyecto gatsby, es usando un fichero de estilos. nosotros crearemos este archivo en `src/styles/global.css`. Agreguemos nuestros primeros estilos globales.

Para poder importar este fichero de estilos globalmente, necesitamos hacerlo desde otro fichero especial de gatsby llamado `gatsby-browser`. Este archivo tiene que estar en la raíz de nuestro proyecto y sirve para implementar una serie de APIs que nos facilita Gatsby que son ejecutados en el cliente. Veremos estas APIs en otro video mas adelante. Una vez hemos creado el archivo, podemos importar nuestros estilos globales. Veamos el resultado ejecutando el comando `yarn develop`.

También ponemos crear archivos css específicos de componentes. para mostrarlo, crearemos un componente Title. lo creamos en `scr/components/title.js`. ahora creamos el archivo css especifico de este componente, le llamamos `title.css`. para conectar ambos archivos, importamos `title.css` en `title.js` y añadimos la clase `title` a el tag H1.

Veamos como se ve nuestro nuevo componente title en la pagina principal.

- global styles
- crear gatsby project
- crear global.css (src/styles/global.css) (puede agregarse donde quieras.)

```css
html {
  background-color: lavenderblush;
}
```

- crear gatsby-browser.js (./gatsby-browser.js)

```js
import "./src/styles/global.css";

// or:
// require('./src/styles/global.css')
```

- estilos para un componente
- creamos title.js (src/components/title.js)
- creamos title.css (src/components/title.css)
- creamos el componente title (h1) y importamos los estilos
- agregamos estilos básicos en title.css

## CSS en JS

- gatsby new emotion-tutorial https://github.com/gatsbyjs/gatsby-starter-hello-world
- cd emotion-tutorial
- yarn add gatsby-plugin-emotion @emotion/core @emotion/styled
- en el `gatsby-config.js`:

```js
module.exports = {
  plugins: [`gatsby-plugin-emotion`]
};
```

- en pages/index.js:

```js
import React from “react”
import styled from “@emotion/styled”
import { css } from “@emotion/core”

const Container = styled.div`
  margin: 3rem auto;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
`
const underline = css`
  text-decoration: underline;
`
export default function Index() {
return (
	<Container>
    <h1 css={underline}>About Emotion</h1>
    <p>Emotion is uber cool</p>
	</Container>
)
```

- create `src/components/layout.js`:

```js
import React from "react";
import { Global, css } from "@emotion/core";
import styled from "@emotion/styled";

const Wrapper = styled("div")`
  border: 2px solid green;
  padding: 10px;
`;

export default ({ children }) => (
  <Wrapper>
    <Global
      styles={css`
        div {
          background: red;
          color: white;
        }
      `}
    />
    {children}
  </Wrapper>
);
```

- update `pages/index.js`:

```js
import Layout from “../components/layout”
export default () => <Layout>Hello world!</Layout>
```

CSS en JS es un enfoque en el que los estilos se escriben en archivos JavaScript en lugar de en archivos CSS externos para encapsular fácilmente los estilos por componentes, eliminar el código muerto, con un rendimiento más alto, estilos dinámicos, y más.

CSS-in-JS, aunque no es obligatorio en Gatsby, es muy popular entre los desarrolladores de JavaScript por los motivos mencionados anteriormente.

Ten en cuenta que esta funcionalidad no forma parte de React o Gatsby, y requiere el uso de cualquiera de las muchas librerias CSS-in-JS de terceros. Veremos como configurar las dos librerias mas famosas: Emotion y Styled-components. Empezamos mostrando como se configura un proyecto gatsby con Emotion.

- crearemos un proyecto desde cero, utilizando el starter mas básico: “hello world”.
- Luego instalaremos todas las dependencias necesarias para usar emotion en un proyecto gatsby: - gatsby-plugin-emotion - @emotion/core
- @emotion/styled también la usaremos, pero no es obligatoria.
- ahora abrimos el archivo `gatsby-config.js` para configurar el plugin de emotion en nuestro proyecto.
- Ya tenemos todo configurado. es hora de crear nuestros primeros componentes con emotion.
- Si no has usado nunca una librería como emotion, inicialmente puede resultarte un poco extraño. Hay muchas formas de escribir componentes con emotion, así que puedes escoger la manera que mejor de convenga.
- para ver el resultado, ejecutamos el comando `gatsby develop`

- Con emotion, No solo puedes crear css por componente, también puedes crear css global. creemos un componente llamado `layout.js`
- Emotion nos brinda un componente especial, el cual una vez renderizado, agrega css global en nuestra pagina. este componente se llama `Global`, y lo puedes importar desde `@emortion-core`
- para ver este componente en acción, vayamos a nuestra pagina y encapsulemos todo dentro de este nuevo componente.

### Layout component

-
