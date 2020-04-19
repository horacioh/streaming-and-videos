# Hacer un site con autenticacion de usuario

## Pasos

- Vamos a crear un site que tenga una parte privada bajo autenticacion. Creemos un nuevo proyecto desde cero con la plantilla hello-world

  - gatsby new gatsby-con-autenticacion gatsbyjs/gatsby-starter-hello-world
  - cd gatsby-auth

- Para este proyecto, necesitaremos crear varios archivos. los ire explicando mientras los creo. el primero que crearemos sera la barra de navegacion, en donde tendremos los links necesarios para poder probar la autenticacion.

- de momento pondremos links a la pagina principal, y al final lo modificaremos para poder probarlo.

```js
// src/components/nav-bar.js

import React from "react";
import { Link } from "gatsby";

export default () => (
  <div
    style={{
      display: "flex",
      flex: "1",
      justifyContent: "space-between",
      borderBottom: "1px solid #d1c1e0",
    }}
  >
    <span>No estan logados</span>

    <nav>
      <Link to="/">Inicio</Link>
      {` `}
      <Link to="/">Perfil</Link>
      {` `}
      <Link to="/">Cerrar Sesion</Link>
    </nav>
  </div>
);
```

- ahora crearemos un layout que nos servira para añadir la barra de navegacion en todas las paginas

```js
// src/components/layout.js

import React from "react";

import NavBar from "./nav-bar";

const Layout = ({ children }) => (
  <>
    <NavBar />
    {children}
  </>
);

export default Layout;
```

- y modificamos la pagina principal para que use nuestro nuevo layout

```js
import React from "react";

import Layout from "../components/layout";

export default () => (
  <Layout>
    <h1>Hello world!</h1>
  </Layout>
);
```

- ahora pasamos a crear nuestro servivcio de autenticacion. para ello crearemos el archivo `auth.js` en `src/services`.
- por ser un tutorial corto, usaremos un sistema de autenticacion simple con las claves codificadas en el mismo fichero, pero esto no es recomendado para una aplicacion en produccion
- para mas informacion de como agregar autenticacion realmente en una aplicacion Gatsby, puedes ver esta otra guia mas detallada (https://www.gatsbyjs.org/docs/building-a-site-with-authentication/)

```js
// src/services/auth.js

export const isBrowser = () => typeof window !== "undefined";

export const getUser = () =>
  isBrowser() && window.localStorage.getItem("gatsbyUser")
    ? JSON.parse(window.localStorage.getItem("gatsbyUser"))
    : {};

const setUser = (user) =>
  window.localStorage.setItem("gatsbyUser", JSON.stringify(user));

export const handleLogin = ({ username, password }) => {
  if (username === `john` && password === `pass`) {
    return setUser({
      username: `john`,
      name: `Johnny`,
      email: `johnny@example.org`,
    });
  }

  return false;
};

export const isLoggedIn = () => {
  const user = getUser();

  return !!user.username;
};

export const logout = (callback) => {
  setUser({});
  callback();
};
```

- Al inicio del tutorial usamos la plantilla `hello-world`, que incluye el paquete `@reach/router`. Ahora usando este paquete, podremos crear rutas que solo puedan ver usuarlos logados. Este paquete lo usa Gatsby internamente, asi que no hay necesidad de instalarlo. Pero tenemos que decirle a gatsby que rutas funcionan solamente en el cliente, para que no intente crear paginas estaticas de las rutas autenticadas. para ello, crearemos el fichero `gatsby-node.js`
- en este fichero, implementaremos la API `onCreatePage`, y diremos que todas las rutas que empiecen con `/app/`, son paginas autenticadas y seran creadas bajo demanda.

```js
// gatsby-node.js

// Implement the Gatsby API “onCreatePage”. This is
// called after every page is created.
exports.onCreatePage = async ({ page, actions }) => {
  const { createPage } = actions;

  // page.matchPath is a special key that's used for matching pages
  // only on the client.
  if (page.path.match(/^\/app/)) {
    page.matchPath = "/app/*";

    // Update the page.
    createPage(page);
  }
};
```

- Otra manera de especificar esto, es usando el plugin `gatsby-plugin-create-client-paths` y tendremos el mismo resultado

  - mostrar el plugin: https://www.gatsbyjs.org/packages/gatsby-plugin-create-client-paths

- ahora podemos pasar a crear la pagina `app.js`

```js
// src/pages/app.js

import React from "react";
import { Router } from "@reach/router";
import Layout from "../components/layout";
import Profile from "../components/profile";
import Login from "../components/login";

const App = () => (
  <Layout>
    <Router>
      <Profile path="/app/profile" />
      <Login path="/app/login" />
    </Router>
  </Layout>
);

export default App;
```

- tambien nos hace falta crear los componentes `profile.js`

```js
// src/components/profile.js

import React from "react";

const Profile = () => (
  <>
    <h1>Your profile</h1>
    <ul>
      <li>Name: Your name will appear here</li>
      <li>E-mail: And here goes the mail</li>
    </ul>
  </>
);

export default Profile;
```

- y `login.js`
- aqui tendremos el formulario de login

```js
// src/components/login.js

import React from "react"
import { navigate } from "gatsby"
import { handleLogin, isLoggedIn } from "../services/auth"

export default function Login() {
  const [ username, setUsername] = React.useState("");
  const [ password, setPassword] = React.useState("");

  handleSubmit = event => {
    event.preventDefault()
    handleLogin({ username, password})
  }


    if (isLoggedIn()) {
      navigate(`/app/profile`)
    }

    return (
      <>
        <h1>Log in</h1>
        <form
          method="post"
          onSubmit={event => {
            handleSubmit(event)
            navigate(`/app/profile`)
          }}
        >
          <label>
            Usuario
            <input type="text" name="username" onChange={e => setUsername(e.target.value)} />
          </label>
          <label>
            Password
            <input
              type="password"
              name="password"
              onChange={e => setPassword(e.target.value)}
            />
          </label>
          <input type="submit" value="Log In" />
        </form>
      </>
    )
  }
}
```

- la manera que comprobaremos si un usuario puede acceder a una ruta en concreto, sera englobando la ruta privada en el componente `rutaPrivada` que crearemos a continuacion

```js
// src/components/privateRoute.js

import React, { Component } from "react";
import { navigate } from "gatsby";
import { isLoggedIn } from "../services/auth";

const PrivateRoute = ({ component: Component, location, ...rest }) => {
  if (!isLoggedIn() && location.pathname !== `/app/login`) {
    navigate("/app/login");
    return null;
  }

  return <Component {...rest} />;
};

export default PrivateRoute;
```

- ahora podemos modificar `app.js` para usar este nuevo componente para la ruta del Perfil
  - editar `app.js`

```js
<PrivateRoute path="/app/profile" component={Profile} />
```

- ahora toca modificar algunos componentes para usar nuestro servicio de autenticacion y las nuevas rutas. vayamos primero a la barra de navegacion

  - refactor `nav-bar`

- aqui mostraremos un mensaje diferente si el usuario esta logado o no, ademas de mostrar el boton de cerrar sesion si el usuario esta logado.

```js
import React from "react";
import { Link, navigate } from "gatsby";
import { getUser, isLoggedIn, logout } from "../services/auth";

export default () => {
  let greetingMessage = "";
  if (isLoggedIn()) {
    greetingMessage = `Hello ${getUser().name}`;
  } else {
    greetingMessage = "You are not logged in";
  }
  return (
    <div
      style={{
        display: "flex",
        flex: "1",
        justifyContent: "space-between",
        borderBottom: "1px solid #d1c1e0",
      }}
    >
      <span>{greetingMessage}</span>
      <nav>
        <Link to="/">Home</Link>
        {` `}
        <Link to="/app/profile">Profile</Link>
        {` `}
        {isLoggedIn() ? (
          <a
            href="/"
            onClick={(event) => {
              event.preventDefault();
              logout(() => navigate(`/app/login`));
            }}
          >
            Logout
          </a>
        ) : null}
      </nav>
    </div>
  );
};
```

- hacemos algo parecido en la pagina principal de nuestra aplicacion
  - refactor `pages/index`

```js
import React from "react";
import { Link } from "gatsby";
import { getUser, isLoggedIn } from "../services/auth";

import Layout from "../components/layout";

export default () => (
  <Layout>
    <h1>Hello {isLoggedIn() ? getUser().name : "world"}!</h1>
    <p>
      {isLoggedIn() ? (
        <>
          You are logged in, so check your{" "}
          <Link to="/app/profile">profile</Link>
        </>
      ) : (
        <>
          You should <Link to="/app/login">log in</Link> to see restricted
          content
        </>
      )}
    </p>
  </Layout>
);
```

- y tambien en el componente de perfil
  - refactor `components/profile`

```js
import React from "react";
import { getUser } from "../services/auth";

const Profile = () => (
  <>
    <h1>Your profile</h1>
    <ul>
      <li>Name: {getUser().name}</li>
      <li>E-mail: {getUser().email}</li>
    </ul>
  </>
);

export default Profile;
```
