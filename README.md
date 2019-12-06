# Reforzamiento de Angular

Esta sección es muy importante para comprender el corazón de cómo funciona ionic hoy en día, hay temas que posiblemente sean comunes para ustedes, pero este reforzamiento les ayudará a comprender mejor ionic y Angular en general.

Los temas puntuales son:

1. Idea de los componentes de Angular

2. Componentes hijos y padres

3. Enviar información entre componentes

4. Navegación mediante el RouterModule

5. Módulos en Angular

6. LazyLoad

7. Servicios

8. Consumo de APIS

9. Estructura del proyecto de Angular

10. Uso de servicios de forma global

Estos conceptos son usados en casi toda aplicación de ionic hoy en día, por lo que conocerlos ayudará a que el código de Angular usando en ionic no sea extraño para ustedes.

## Incluir el proyecto Angular en GitHub

```
echo "# ReforzarAngular" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/adolfodelarosades/ReforzarAngular.git
git push -u origin master
```

## Inicio del proyecto - AngularBases

### Crear nuevo proyecto Angular.

`ng new angularbases`

### Ejecutar el proyecto 

`ng serve -o`

Se ejecuta en la URL

`http://localhost:4200/`

### Añadir Bootstrap a index.html

`<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">`

## Componentes básicos

Vamos a crear dos componentes básicos que usaremos en nuestra página, para crear un nuevo componente se usa el comando:

### Crear el componente about

`ng g c pages/about`

Cuando no estemos seguros de lo que va a hacer algun comando podemos añadir `--dry-run` y nos dira lo que hara el comando al ejecutarlo.

`ng g c pages/about --dry-run`

```
CREATE src/app/pages/about/about.component.html (20 bytes)
CREATE src/app/pages/about/about.component.spec.ts (621 bytes)
CREATE src/app/pages/about/about.component.ts (265 bytes)
CREATE src/app/pages/about/about.component.css (0 bytes)
UPDATE src/app/app.module.ts (398 bytes)

NOTE: The "dryRun" flag means no changes were made.
```
Como es exactamente lo que queremos hacer ejecutamos el comando sin esos parámetros, es decir:

`ng g c pages/about`

Dentro de **app/pages/about** se crean todos los archivos del componente, así como también se modifica el archivo **src/app/app.module.ts** para incluir la referencia al nuevo componente.

### Crear el componente contact

`ng g c pages/contact`

Pasa lo mismo que al crear el componente about.

### Crear el componente home

`ng g c pages/home`

### Componente app.component

El componente `app.component` es el componente inicial que se se carga cuando ejecutamos nuestra aplicación:

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/appcomponent.png">

Vamos a modificar el contenido del archivo app.component.html con el siguiente código:

```js
<h1>App Component</h1>

<app-home></app-home>
<app-about></app-about>
<app-contact></app-contact>
```
<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/appcomponentmod.png">


## Implementar las rutas de nuestra aplicación

### Crear el Módulo de Rutas

Vamos a implementar un sistema de rutas par nuestra aplicación. Dentro del directorio *app* vamos a crear un manejador de las rutas, se puede hacer mediante CLI pero vamos a crearlo manualmente, con el comando:

`ng g m appRouting --dry-run`

Nos dice:

`CREATE src/app/app-routing/app-routing.module.ts`

que va a crear el archivo dentro de la carpeta `app-routing` pero nosotros no queremos eso, queremos que directamente este en **app**. Intentemos con:

`ng g m appRouting --flat --dry-run`

Nos indica:

`CREATE src/app/app-routing.module.ts`

que el archivo será creado directamente en **app**, con `--flat` no nos crea el directorio. Por lo que el comando correcto a usar es: 

`ng g m appRouting --flat`

Con esto me crea el archivo `app-routing.module.ts` el cual debemos abrir. 

### Añadir nuestras Rutas

Importaremos las `Routes` que es un tipo manejado por Angular que nos permite implementar las rutas y metetemos una ruta por cada página (componente) que tengamos así como una para cualquier ruta que se meta y no exista que nos lleve a `home`:

```js
import { Routes } from '@angular/router';
import { HomeComponent } from './pages/home/home.component';
import { AboutComponent } from './pages/about/about.component';
import { ContactComponent } from './pages/contact/contact.component';

const routes: Routes = [
  {
    path: 'home',
    component: HomeComponent
  },
  {
    path: 'about',
    component: AboutComponent
  },
  {
    path: 'contact',
    component: ContactComponent
  }
  {
    path: '**',
    redirectTo: 'home'
  }
];
```
Tenemos tambien la directiva `@NgModule`
```js
@NgModule({
  declarations: [],
  imports: [
    CommonModule
  ]
})
export class AppRoutingModule { }
```
El `CommonModule` nos permite usar entre otras cosas directivas como `*ngIf` o `*ngFor` pero eso no lo vamos a usar aquí en la gegestión de las rutas, por lo que lo eliminaremos. 
Lo que usaremos es:

```js
@NgModule({
  imports: [
    RouterModule.forRoot( routes )
  ],
  exports:[
    RouterModule
  ]
})
export class AppRoutingModule { }
```
El `RouterModule.forRoot()` nos indica que usaremos el *Sistema de Rutas Principales* generalmente solo existe un sistema principal de rutas, aquí solo estamos indicando que este archivo usa el `RouterModule`, para que Angular pueda usar el archivo lo exportamos.

Para que angular identifique que el anterior es mi archivo de configuración de rutas necesitamos importarlo en el `app.module.ts` que es el modulo principal de la aplicación, lo colocamos en los `imports`:

```js
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
```

Si recargamos la pagina nos redirigue a `http://localhost:4200/home`, si escribimos cualquier cosa como: `http://localhost:4200/gggggssss` nos redirigue a `http://localhost:4200/home`, si escribimos `http://localhost:4200/contact` funciona pero sigue mostrando lo mismo que cuando cargamos la `home`, esto pasa por que en el `app.component.html` le estamos que renderice esto:

```js
<h1>App Component</h1>

<app-home></app-home>
<app-about></app-about>
<app-contact></app-contact>
```

### Uso de nuestro Módulo de Rutas

En la aplicación no le estamos diciendo donde renderizar estos componentes, para esto vamos a cambiar el código  y usaremos el elemente `router-outlet`, cambiamos el código anterior por este:

```js
<h1>App Component</h1>

<router-outlet></router-outlet>
```

Al cargar de nuevo la aplicación, cada componente se cargara cuando se ponga su correspondiente ruta.

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/home.png">
<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/contact.png">
<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/about.png">

## RouterLink y MenuComponent

### Crear el componente menu

Vamos a crear una funcionalidad de menú sencilla, vamos a crear un componente que no representará una página, sino un componente independiente:

`ng g c components/menu`

Creamos un componente `c` en la carpeta `components` y se llamará `menu`. Se nos indican las acciones realizadas:

```
CREATE src/app/components/menu/menu.component.html (19 bytes)
CREATE src/app/components/menu/menu.component.spec.ts (614 bytes)
CREATE src/app/components/menu/menu.component.ts (261 bytes)
CREATE src/app/components/menu/menu.component.css (0 bytes)
UPDATE src/app/app.module.ts (734 bytes)
```

### Añadir las opciones del menú

Abrimos el archivo `menu.component.html` y vemos su contenido:

```js
<p>menu works!</p>
```
Lo cambiaremos por:

```js
<ul class="list-group">
  <a routerLink="/home"
     class="list-group-item">
     Home
  </a>
  <a routerLink="/about"
     class="list-group-item">
     About
  </a>
  <a routerLink="/contact"
     class="list-group-item">
     Contact
  </a>
</ul>
```

### Renderizar el Ménu

Ya tenemos nuestro componente `menu` pero para que pueda ser renderizado lo metemos en nuestro `app.component.html`:

```js
<h1>App Component</h1>

<app-menu></app-menu>
<router-outlet></router-outlet>
```

Por lo que nuestra pantalla se vera así:

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/menu.png">

### Crear el Ménu Dinamicamente

La anterior es una forma de hacer nuestro menú, poniendo cada una de nuestras opciones del menú en el archivo html. Vamos a hacer algunos cambios, abrimos el archivo `menu.component.ts` donde insertaremos un array de objetos que representan  cada una de las rutas del menú:

```js
rutas = [
  {
    name: 'Home',
    path: '/home'
  },
  {
    name: 'About',
    path: '/about'
  },
  {
    name: 'Contact',
    path: '/contact'
  }
];
```

Como este array lo estamos declarando en nuestro archivo `ts` lo podemos usar en el archivo `html` para cargar nuestras rutas dinámicamente:

```js
<ul class="list-group">
  <a routerLink="{{ ruta.path }}"
     class="list-group-item"
     *ngFor="let ruta of rutas">
     {{ ruta.name }}
  </a>
</ul>
```

Si cargamos la página nuevamente el menú sigue viendose igual pero hemos optimizado el código, la anterior es una forma de inyectar los valores, pero existe otra forma diferente de inyectarlos en los atributos que en este caso se puede usar con el atributo `routerLink`:

```js
<ul class="list-group">
  <a [routerLink]="ruta.path"
    class="list-group-item"
    *ngFor="let ruta of rutas">
    {{ ruta.name }}
  </a>
</ul>
```

Cuando usamos los corchetes lo que estamos haciendo es enlazando el valor de la variable al elemento.

Nuevamente si cargamos el sitio el menú sigue funcionando corrrectamente.

## Módulo de páginas

A medida que hemos ido añadiendo componentes estos se han ido insertando en el `app.module.ts`:

```js
declarations: [
  AppComponent,
  AboutComponent,
  ContactComponent,
  HomeComponent,
  MenuComponent
],
```
Si crearamos más componentes estos se irian insertando aquí, lo ideal es que el `app.modulo.ts` estuviera lo más limpio posible y facil de leer, los componentes anteriores podrian estar organizado en otro lugar, eso es lo que haremos aquí , crear un modulo que nos permita agrupar todas las páginas.

### Crear el modulo pages

En pages nos crearemos un nuevo modulo:

`ng g m pages/pages --flat`

Le decimos que nos cree un modulo `m` en el directorio `pages` y que se llame `pages` y que no cree ninguna carpeta.

Nos indica que ha sido creado `CREATE src/app/pages/pages.module.ts (191 bytes)`.

### Eliminar las pages de app.module.ts

La idea es que `Home`, `About` y `Contact` no esten en `app.module.ts`, quitemoslos.

```js
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { AppRoutingModule } from './app-routing.module';
import { MenuComponent } from './components/menu/menu.component';

@NgModule({
  declarations: [
    AppComponent,
    MenuComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
Al ejecutar nuevamente la aplicación nos marca una serie de errores y las páginas ya no van.

```
Uncaught Error: Component HomeComponent is not part of any NgModule or the module has not been imported into your module.
    at JitCompiler._createCompiledHostTemplate (compiler.js:25915)
```

### Incluir las pages en pages.module.ts

Para arreglar este desastre nos vamos a `pages.module.ts` veamos su contenido:

```js
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

@NgModule({
  declarations: [],
  imports: [
    CommonModule
  ]
})
export class PagesModule { }
```

Aquí insertaremos las declaraciones que anteriormente teniamos en el `app.module.ts`, nos quedara así:

```js
import { AboutComponent } from './about/about.component';
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { HomeComponent } from './home/home.component';
import { ContactComponent } from './contact/contact.component';

@NgModule({
  declarations: [
    HomeComponent,
    AboutComponent,
    ContactComponent
  ],
  exports: [
    HomeComponent,
    AboutComponent,
    ContactComponent
  ],
  imports: [
    CommonModule
  ]
})
export class PagesModule { }
```

### Importar pages.module dentro de app.module.ts

Hasta aquí el `pages.module.ts` ya conoce cuales son los componentes declarados, para que la aplicación de Angular sepa que esos componentes existen, tengo que importar el `pages.modulo.ts` en mi `app.module.ts` se coloca en los `imports`:

```js
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { AppRoutingModule } from './app-routing.module';
import { MenuComponent } from './components/menu/menu.component';
import { PagesModule } from './pages/pages.module';

@NgModule({
  declarations: [
    AppComponent,
    MenuComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    PagesModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

**Si volvemos a cargar la página los errores siguen mostrandose, este es un pequeño error que tiene Angular CLI que no refresca bien cuando creamos modulos, la forma de solucionarlo es dar de baja el servidor y volverlo a cargar.**

Una vez que se cargo nuevamente el servidor ya podemos ver nuestra aplicación sin errores, nuevamente puedo navegar a `Home`, `About` y `Contact`. Funciona mi aplicación con la diferencia de que ahora tengo un `PagesModule`, todas las nuevas páginas que tenga las puedo declarar en el `pages.module` en lugar del `app.module` eviatndo tocarlo para nuevas páginas. Pero aun con esta mejora sigue siendo un poco engorrozo ya que por cada nueva página la debo incluir en el `pages.module` tanto en el la declaración como en la exportación, y ademas cuando lo cargamos de esta manera todos los modulos forman parte del `Bundle` principal, es decir que cuando se genera la aplicación de producción todos estos archivos se compactaran en un unico archivo final.

## LazyLoad de PostsComponent

En esta sección vamos a crear un nuevo componente el cual va a ser cargado de manera perezosa. utilizando LazyLoad, incluso la ruta también puede ser cargada de manera perezosa.

### Crear carpeta posts dentro de pages

Dentro de esta carpeta vamos a crear todo lo relacionado a los posteos que hagamos. 

### Crear modulo posts y su archivo de rutas

Vamos a crear un modulo posts (lo de la carpeta anterior estaria de más usando el siguiente comando) `ng g m pages/posts` vamos agregar la bandera `--routing`
para que me cree un archivo de configuración de rutas, podemos agregar `--dry-run` para asegurarme de que todo esto va a crearse:

`ng g m pages/posts --routing --dry-run`

Nos indica:

```
CREATE src/app/pages/posts/posts-routing.module.ts (249 bytes)
CREATE src/app/pages/posts/posts.module.ts (276 bytes)

NOTE: The "dryRun" flag means no changes were made.
```

Como es exactamente lo que queremos hacer ejecutamos el comando sin el `--dry-run`:

`ng g m pages/posts --routing`

Nos indica:

```
CREATE src/app/pages/posts/posts-routing.module.ts (249 bytes)
CREATE src/app/pages/posts/posts.module.ts (276 bytes)
```
Los dos archivos han sido creados.

El archivo `posts.module.ts` tiene una importación del `PostsRoutingModule`:

```js
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

import { PostsRoutingModule } from './posts-routing.module';


@NgModule({
  declarations: [],
  imports: [
    CommonModule,
    PostsRoutingModule
  ]
})
export class PostsModule { }
```

Si vemos el archivo `posts-routing.module.ts` es muy similar al archivo de rutas que creamos manualmente, con la única de diferencia que en lugar de usar `forRoot` usa `forChild` por ser una ruta hija.

```js
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';


const routes: Routes = [];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class PostsRoutingModule { }
```

### Crear el componente posts

Vamos a crear el componente posts:

`ng g c pages/posts --dry-run`

Nos indica:

```
CREATE src/app/pages/posts/posts.component.html (20 bytes)
CREATE src/app/pages/posts/posts.component.spec.ts (621 bytes)
CREATE src/app/pages/posts/posts.component.ts (265 bytes)
CREATE src/app/pages/posts/posts.component.css (0 bytes)
UPDATE src/app/pages/posts/posts.module.ts (342 bytes)

NOTE: The "dryRun" flag means no changes were made.
```

como es lo que queremos ejecutamos el comando sin `--dry-run`

`ng g c pages/posts`

Se han creado los 4 archivos dentro de la carpeta posts junto con los dos archivos que habiamos creado del modulo y la ruta (como trabaja Ionic).

Nos indica:

```
CREATE src/app/pages/posts/posts.component.html (20 bytes)
CREATE src/app/pages/posts/posts.component.spec.ts (621 bytes)
CREATE src/app/pages/posts/posts.component.ts (265 bytes)
CREATE src/app/pages/posts/posts.component.css (0 bytes)
UPDATE src/app/pages/posts/posts.module.ts (342 bytes)
```

Notese que indica que el archivo `posts.module.ts` ha sido modificado para insertar la declaración del componente `declarations: [PostsComponent],`.

### Añadir ruta en nuestro `posts-routing.module.ts`

Añadamos el siguiente código:

```
import { PostsComponent } from './posts.component';

const routes: Routes = [
  {
    path: '',
    component: PostsComponent
  }
];
```
Cual debería ser el `path` que tengamos aquí, asumamos que queramos tener algo así como: `localhost:4200/posts` podriamos pensar que tendriamos que poner `path: 'posts',` en teoría no esta mal pero esta es una **ruta hija**, **lo que realmente queremos hacer es que todo este modulo con su definicion de rutas y componentes sea cargado de manera perezosa**, y que sea cargado cuando sea necesario, y ¿cuando es necesario? 

### Incluir de manera perezosa nuestro modulo posts en el app-routing.module

Vamos a definirnos una nueva ruta en `app-routing.module.ts` para hacer referencia a los `posts`:

```
{
  path: 'posts',
  component: PostComponent
},
```
Si lo hacemos de esta manera, en primera nos indica que `Cannot find name 'PostComponent'.ts(2304)`, pero sobre todo no estariamos cargando nuestro modulo de manera perezosa que es realmente lo que queremos, por lo que el componente no iria aquí. 

Lo que queremos realmete es cargar el modulo que tiene toda la información de los `posts`, lo tendriamos que hacer de la siguiente forma:

```
{
  path: 'posts',
  loadChildren: './pages/posts/posts.module#PostsModule'
},
```
`loadChildren` lo que indica es que vamos a usar la carga perezosa o LazyLoad, para cargar el modulo `PostsModule` que se encuentra en la ruta `./pages/posts/posts.module`.

### Añadir la ruta posts al menu

En el archivo `menu.component.ts` añadimos la nueva opción de menú:

```
{
  name: 'Posts',
  path: '/posts'
}
```
Si probamos la navegación a `Posts` nos carga el componente:

**En caso de no ser cargado damos de baja el server y lo volvemos a cargar. (Por que creamos un nuevo modulo)**.

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/posts.png">

Como realmente podemos saber si estamos cargando de manera perezosa el componente `Posts`, como habiamos dicho anteriormente al usar `loadChildren` indica que vamos a usar LazyLoad. Para terminar de comprobarlo abramos las Herramientas de Desarrollador especificamente en la ventana Network y recargamos la opción de menú `Home`, tendremos una pantalla como la siguiente:

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/networkhome.png">

Como se puede observar el archivo `vendor.js` es el que más tarda en cargar por que contiene varias cosas entre y entre otras cosas por que estamos en modo de desarrollo, se podra optimizar para modo producción. 

Si entramos a la opción de `Posts`:

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/networkposts.png">

Detecta que necesesitamos cargar todo lo referente al modulo de posts y carga el archivo `pages-posts-posts-module.js` donde viene todo lo referente a ese modulo entre ello el modulo de rutas y el componente `posts`.

**Todo esto que hemos tenido que hacer manualmente para generear la carga perezosa o LazyLoad en Ionic lo hace automaticamente, teniendo el beneficio de que la aplicación es mucho más ligera y cuando cargue la primera pantalla posiblemente solo cargue la primera página mediante LazyLoad y la aplicación se va a desplegar en el dispositivo sumamente rapida.**





