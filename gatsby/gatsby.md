# Gatsby

Gatsby is a blazing fast modern site generator for React. Check out their amazing docs [here](https://www.gatsbyjs.org/docs/)

## Content

- Setup your environment [🔗](#setup)
- Building blocks [🔗](#buiding-blocks)
- Styling in Gatsby [🔗](#styling-in-gatsby)
- Gatbsy Plugins [🔗](#gatsby-plugins)
- Layout [🔗](#layout)
- Data en Gatsby [🔗](#data-in-gatsby)
- Gatbsy Themes [🔗](#gatsby-themes)

## Setup

- Familiarize with the command line
- Install Nodejs
- install the gatsby CLI
- install Git
- create your first gatsby site: `gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world` (explicar el command)
- run your site locally

## Building Blocks

- explain the command `gatsby new`: `gatsby new [SITE_DIRECTORY_NAME] [URL_OF_STARTER_GITHUB_REPO]`
  - If you omit a URL from the end, Gatsby will automatically generate a site for you based on the [default starter](https://github.com/gatsbyjs/gatsby-starter-default).
- `yarn develop` and open the repo
- mostrar como haciendo cambios en el codigo se recarga la pagina (HMR)
  - Hello world => Hola Gatsby
  - inline styles
  - img: `<img src="https://source.unsplash.com/random/400x200" alt="" />`
- hablar sobre las paginas
- mostrar como reusar componentes en paginas
- links entre paginas (`<Link />`)

## Styling in Gatsby

- archivo `global.css` y agregando requires en `gatsby-browser`
- CSS Modules (solo mencionarlo)
- CSS-in-JS (emotion o styled-components)
- Mencionar tambien el soporte para TypographyJS, Sass, JSS, Stylus y PostCSS

## Gatsby Plugins

- es la forma en la que Gatsby tiene para _extender_ y compartir funcionalidades entre proyectos.
- Un plugin es un simple paquete de npm que puedes instalar en tu proyecto gatsby.
- Instalemos `gatsby-plugin-typography`

  - `npm install --save gatsby-plugin-typography react-typography typography typography-theme-fairy-gates`
  - mensionar que los plugins habitualmente tienen dependencias de otros paquetes que tambien hay que instalar.
  - agregar plugin en el `gatsby-config.js`

  ```js
  module.exports = {
    plugins: [
      {
        resolve: `gatsby-plugin-typography`,
        options: {
          pathToConfigModule: `src/utils/typography`
        }
      }
    ]
  };
  ```

  - typography.js necesita un fichero de configuracion que debemos agregar en `src/utils/typography.js`:

  ```js
  import Typography from "typography";
  import fairyGateTheme from "typography-theme-fairy-gates";
  const typography = new Typography(fairyGateTheme);
  export const { scale, rhythm, options } = typography;
  export default typography;
  ```

  - `gatsby develop`
  - comprobar que en el head de nuestra pagina este el tag `<style>` agregado por el plugin:

  ![typography.js style tag](https://www.gatsbyjs.org/static/2c34a22832b1dd0828ac87fde925622a/1351f/typography-styles.png)

## Layout

- Para que son los layout components? partes comunes de las paginas.
- creacion de un componente layout.

## Data in Gatsby

- GraphQl Elevator pitch (link a la serie)
- Explicar como Gatsby usa GraphQL para obtener informacion

## Gatsby Themes

## Deploy your Gatsby site

- gatsby new ejemplo-produccion
- open package json
- mostrar los comandos
- `yarn build`
- `yarn serve`
- audit lighthouse
- open gatsby-config
- gatsby-plugin-manifest
- gatsby-plugin-offline
- mostrarlos en el package.json
- metadatos con react-helmet
- mostrar SEO component. metadatos basicos ya seteados. tambien puedes pasarle props (mostrar props)
