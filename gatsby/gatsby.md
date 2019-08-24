# Gatsby

Gatsby is a blazing fast modern site generator for React. Check out their amazing docs [here](https://www.gatsbyjs.org/docs/)

## Content

- Setup your environment [🔗](#setup)
- Building blocks [🔗](#buiding-blocks)
- Styling in Gatsby [🔗](#styling-in-gatsby)
- Layout [🔗](#layout)

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

## Layout

- creacion de un componente layout.

## Data in Gatsby

- GraphQl Elevator pitch (link a la serie)
- Explicar como Gatsby usa GraphQL para obtener informacion
