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

```js
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

```js
{
  path: 'posts',
  component: PostComponent
},
```
Si lo hacemos de esta manera, en primera nos indica que `Cannot find name 'PostComponent'.ts(2304)`, pero sobre todo no estariamos cargando nuestro modulo de manera perezosa que es realmente lo que queremos, por lo que el componente no iria aquí. 

Lo que queremos realmete es cargar el modulo que tiene toda la información de los `posts`, lo tendriamos que hacer de la siguiente forma:

```js
{
  path: 'posts',
  loadChildren: './pages/posts/posts.module#PostsModule'
},
```
`loadChildren` lo que indica es que vamos a usar la carga perezosa o LazyLoad, para cargar el modulo `PostsModule` que se encuentra en la ruta `./pages/posts/posts.module`.

### Añadir la ruta posts al menu

En el archivo `menu.component.ts` añadimos la nueva opción de menú:

```js
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

## Servicios y data externa

### Editando nuestro componente posts para que muestre información fija

Vamos a abrir el archivo `posts.component.html` su contenido actual es:
```
<p>posts works!</p>
```
vamos a cambiarlo por el siguiente:

```js
<h1>Posts</h1>
<ul class="list-group">
  <li *ngFor="let post of [1,2,3,4,5]" class="list-group-item">
    <h3>Título</h3>
    <p>Lorem ipsum dolor sit, amet consectetur adipisicing elit. Non necessitatibus incidunt voluptates, reiciendis id magnam reprehenderit recusandae maiores eum nisi facilis beatae corrupti saepe deserunt itaque molestias modi molestiae possimus!</p>
  </li>
</ul>
```

Lo que obtenemos es:

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/postsItemsFijos.png">


### URL del servidor

Lo ideal es que la información la recuperaramos de algun servicio, tenemos el URL:

`https://jsonplaceholder.typicode.com/posts` 

el cual nos regresa varios posts con los siguientes datos:

```json
{
  "userId": 1,
  "id": 1,
  "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
  "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
},
```

### Creación del servicio

Cuando nosotros queremos consumir este tipo de data lo mejor de todo es centralizarlo todo el lo que se conoce como `service` el cual nos permite compartir toda esta información en todos los componentes que lo requieran. Angular lo incluye de manera global con una instrucción:

`ng g s service/data --spec=false`

Estamos diciendo que nos cree un servicio `s` en la carpeta `service` llamado `data` y con `--spec=false` estamos indicando que no nos cree el archivo de pruebas.

Nos indica:

```
Option "spec" is deprecated: Use "skipTests" instead.
CREATE src/app/service/data.service.ts (133 bytes)
```

Noten que no modifica el archivo `app.module` por que ahora los sevicios en Angular tienen la propiedad `providedIn: 'root'` `root` indica que el servicio estara disponible de manera global en toda la aplicación. 

Este servicio tendra la lógica para obtener los mensajes del URL `https://jsonplaceholder.typicode.com/posts`.

### Incluir el modulo HttpClientModule en nuestro app.module

Para realizar una petición http a un servidor es necesario importar un módulo, esto lo incluimos en el `app.module.ts`:

`import { HttpClientModule } from '@angular/common/http';`

y lo incluimos en los `imports`:


```js
imports: [
  BrowserModule,
  AppRoutingModule,
  PagesModule,
  HttpClientModule
],
```

### Códificar el servicio para recuperar la información del URL

Regresemos al servicio `data.service.ts`:

```js
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class DataService {

  constructor() { }
}
```

Para poder hacer la petición al servidor necesitamos inyectar algo en el constructor `private http: HttpClient` asegurarse que el `HttpClient` lo importe de `@angular/common/http` por que hay otros tipos. Con esto ya podemos crear un método que me cargue la información, le llamaremos `getPosts()`, por lo que mi `data.service.ts` quedara así:

```js
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

@Injectable({
  providedIn: 'root'
})
export class DataService {

  constructor( private http: HttpClient) { }

  getPosts(){
    return this.http.get('https://jsonplaceholder.typicode.com/posts');
  }
}
```

### Como utilizar el servicio en el componente

#### Inyectar el servicio en post.component.ts y llamar al método getPosts()

Para utilizar el servicio que acabamos de hacer, abrimos el `post.component.ts`, debemos inyectar el servicio que acabamos de hacer en el contructor, ademas en el método `ngOnInit()` que es el método que se ejecuta cuando la página es cargada por primera vez, llamaremos al método que creamos en el servicio:

```js
import { Component, OnInit } from '@angular/core';
import { DataService } from 'src/app/service/data.service';

@Component({
  selector: 'app-posts',
  templateUrl: './posts.component.html',
  styleUrls: ['./posts.component.css']
})
export class PostsComponent implements OnInit {

  constructor( private dataService: DataService ) { }

  ngOnInit() {
    this.dataService.getPosts()
      .subscribe( posts => {
        console.log(posts);
      });
  }
}
```

En teoria `this.dataService.getPosts()` llamaría al método `getPosts()` del servicio, pero `getPosts()` realmente retorna un observable, ese observable trae la información que nos interesa, pero para poder obtener esa información debemos suscribirnos (`.subscribe`), una vez suscrito el resultado se almacena en `posts`, utilizando una función de flecha imprimimos la información que nos retorna en la consola.

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/getPosts.png">

Podemos observar como recupera hasta 100 post.

## Mostrar POSTS en el componente

Vamos a crear una variable `mensajes` de tipo `any[]` donde vamos a amacenar los posts recibidos `this.mensajes = posts;`, como esto nos da un error debemos declarar a `posts` de ese tipo `.subscribe( (posts: any[] ) => {`, nuestro código queda así:

```js
import { Component, OnInit } from '@angular/core';
import { DataService } from 'src/app/service/data.service';

@Component({
  selector: 'app-posts',
  templateUrl: './posts.component.html',
  styleUrls: ['./posts.component.css']
})
export class PostsComponent implements OnInit {

  mensajes: any[] = [];

  constructor( private dataService: DataService ) { }

  ngOnInit() {
    this.dataService.getPosts()
      .subscribe( (posts: any[] ) => {
        console.log(posts);
        this.mensajes = posts;
      });
  }
}
```

Con esto ya podemos parar al archivo `posts.component.html` y sustituir el array `[1,2,3,4,5]` por `mensajes`, con este pequeño cambio ya se pintan en pantalla los 100 posts recuperados. Vamos a hacer algunos otros ajustes para mostrar bien toda la información, el código queda asi:

```js
<h1>Posts</h1>
<ul class="list-group">
  <li *ngFor="let mensaje of mensajes" class="list-group-item">
    <h3>{{ mensaje.title }}</h3>
    <p>{{ mensaje.body }} </p>
  </li>
</ul>

```
<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/getPostsInfo.png">

### Nos suscribimos cada que entramos a Posts

Trabajar de esta forma tiene un pequeño inconveniente, ya que cada que entramos al componente `Posts` nos estamos suscribiendo a un elemento, vamos a poner un mensaje en `ngOnInit` para ver claramente que se dispara cada que entramos en el:

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/getPostsInit.png">

Como se ve claramente en la imagen cada que entramos en la opción `Posts` nos suscribimos nuevamente al elemento, esto puede provocar desperdicio de memoria por que estamos suscribiendo nuevamente sin cancelar la suscripción anterior.

### Uso del Pipe Async para cancelar suscripción antes de abrir otra

Hay varias formas de solucionar esto, una seria asignar la suscripción a una variable y cuando llamemos al método `ngOnDestroy` es decir que cuando la página se va a destruir podemos cancelar la suscripción, para prevenir errores o falta de memoria en caso de que se acceda a `Posts` muchas veces.

Existe otra forma de solucionarlo, angular nos ofrece un `pipe` que automaticamente cancela la suscripción cuando ya no se utiliza, es el `pipe async`. 

Vamos a modificar nuestro código para utilizar dicho pipe, en el archivo `posts.component.ts` vamos a cambiar el código para que quede así:

```js
import { Component, OnInit } from '@angular/core';
import { DataService } from 'src/app/service/data.service';

@Component({
  selector: 'app-posts',
  templateUrl: './posts.component.html',
  styleUrls: ['./posts.component.css']
})
export class PostsComponent implements OnInit {

  mensajes: any;

  constructor( private dataService: DataService ) { }

  ngOnInit() {
    console.log('INIT!');

    this.mensajes = this.dataService.getPosts();
      //.subscribe( (posts: any[] ) => {
      //  console.log(posts);
      //  this.mensajes = posts;
      //});
  }
}
```

1. En primer lugar `mensajes` ya no se declara como array sino solo de tipo `any`, es decir `mensajes: any;`.

2. En segundo lugar ya no nos suscribimos, simplemente igualamos nuestro metodo del servicio a mensajes:

`this.mensajes = this.dataService.getPosts();`

Con esto lo que se almacena en `mensajes` es un apuntador al valor de los posts, es decir de la suscripción que esta regresando. Con estos cambios la página a dejado de funcionar. 

```
PostsComponent.html:1 ERROR Error: Cannot find a differ supporting object '[object Object]' of type 'object'. NgFor only supports binding to Iterables such as Arrays.
```

Nos falta que en nuestro archivo `posts.component.html` pasar los mensajes por el `pipe async`:

```js
<h1>Posts</h1>
<ul class="list-group">
  <li *ngFor="let mensaje of mensajes | async" class="list-group-item">
    <h3>{{ mensaje.title }}</h3>
    <p>{{ mensaje.body }} </p>
  </li>
</ul>
```
Al hacer esto nuestra página vuelve a trabajar, **con la diferencia de que cuando salga del component `Posts` se cancela la suscripción y cuando vuelvo a entrar se vuelve a crear la suscripción** lo que optimiza nuestra página.

**Pero esto tiene un inconveniente, ya no tengo lo que viene en cada uno de los elementos, ya no se pinta:

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/getPostsSinInfo.png">

Podría llamar al URL del servicio `https://jsonplaceholder.typicode.com/posts` y ver los datos, pero si lo quisiera hacer desde mi propia aplicación ¿como puedo mandar esos datos a consola? 

### Uso del operador tap para imprimir datos a consola

Un **tip** para hacerlo es imprimiendolo desde nuestro servicio. 

Abramos nuestro servicio el archivo `data.service.ts` tenemos el método:

```js
getPosts() {
    return this.http.get('https://jsonplaceholder.typicode.com/posts');
  }
```
Como el `return` regresa un observable podemos trabajar con los operadores que ya vienen precargados cuando creamos una aplicación utilizando el angular CLI, solo hay que importarlos.

Existe un elemento en los **[Reactive Extensions Library for JavaScript](https://rxjs-dev.firebaseapp.com/)** llamado `tap` lo importaremos con `import { tap } from 'rxjs/operators';` la función del `tap` es ejecutar una acción cuando se recibe una suscripción o cuando se optiene algun mensaje pero no tiene efectos secundarios, ni tampoco transforma la data. Para utilizarlo vamos aponer los siguiente:

```js
getPosts() {
    return this.http.get('https://jsonplaceholder.typicode.com/posts')
      .pipe(
        tap( posts => {
          console.log(posts);
        })
      );
  }
```

De esta manera ya vuelve a pintar en la consola los `posts` retornados la diferencia es que ahora los hace desde el archivo `data.service.ts` y antes lo hacia desde `posts.component.ts`.

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/getPostsConInfo.png">

Podriamos aun simplificar un poco más el código pero el resultado sería exactmente el mismo:

```js
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';

import { tap } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class DataService {

  constructor( private http: HttpClient) { }

  getPosts() {
    return this.http.get('https://jsonplaceholder.typicode.com/posts')
      .pipe( tap( console.log ) );
  }
}
```

## @Input - Enviar información hacia un componente hijo

### Creación del componente post

Vamos a crear el componente `post` donde pintaremos lo referente a un post, para crearlo escribimos el comando:

`ng g c pages/posts/post --spec=false`

Nos indica:

```
Option "spec" is deprecated: Use "skipTests" instead.
CREATE src/app/pages/posts/post/post.component.html (19 bytes)
CREATE src/app/pages/posts/post/post.component.ts (261 bytes)
CREATE src/app/pages/posts/post/post.component.css (0 bytes)
UPDATE src/app/pages/posts/posts.module.ts (412 bytes)
```
Notese que Angular a detectado que tenemos el modulo `posts.module.ts` y lo ha modificado para incluir el nuevo componente `PostComponent`:

```js
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

import { PostsRoutingModule } from './posts-routing.module';
import { PostsComponent } from './posts.component';
import { PostComponent } from './post/post.component';


@NgModule({
  declarations: [PostsComponent, PostComponent],
  imports: [
    CommonModule,
    PostsRoutingModule
  ]
})
export class PostsModule { }
```

Lo que vamos a hacer el mover todo el elemento `<li>` que tenemos en `posts.component.html` a `post.component.html` haciendo pequeños cambios en ambos archivos:

`posts.component.html`

```js
<h1>Posts</h1>
<ul class="list-group">
  <app-post *ngFor="let mensaje of mensajes | async"></app-post>
  <!--
  <li *ngFor="let mensaje of mensajes | async" class="list-group-item">
    <h3>{{ mensaje.title }}</h3>
    <p>{{ mensaje.body }} </p>
  </li>
  -->
</ul>
```

`post.component.html`

```js
<li class="list-group-item">
  <h3>{{ mensaje.title }}</h3>
  <p>{{ mensaje.body }} </p>
</li>
```
Si cargamos la página se muestra así:

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/getPostsPostError.png">

Intenta mostrar los 100 post pero no reconoce `title` en `post.component.html`, para corregir este fallo debemos mandar datos desde el Padre al Hijo que es lo que no esta pasado ahora mismo. 

### Usando @Input para recibir datos del padre

Vamos a `post.component.ts` vamos a declarar una propiedad que será recibida del exterior, esto se hace con `@Input() mensaje;` el `Input` debe ser importado de `@angular/core`, lo que estamos indicando que la propiedad `mensaje` será recibida desde el componente exterior sino se recibe nada será igual a `null` o `undefined`. Notese que la variable `mensaje` es el mismo nombre que tenemos en el `html`, el archivo `post.component.ts` queda así:

```js
import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'app-post',
  templateUrl: './post.component.html',
  styleUrls: ['./post.component.css']
})
export class PostComponent implements OnInit {

  @Input() mensaje: any;

  constructor() { }

  ngOnInit() {
  }
}
```

Ya estamos listos para recibir la propiedad `mensaje` desde el padre.

### Enviar datos desde el Padre al Hijo

Abrimos el archivo `posts.component.html` ahora tenemos:

```js
<app-post *ngFor="let mensaje of mensajes | async"></app-post>
```

Vamos a cambiarlo para que quede así:

```js
<app-post *ngFor="let mensaje of mensajes | async"
          [mensaje]="mensaje">
</app-post>
```

Entre los corchetes se pone el nombre de la variable que declaramos con `@Input() mensaje` en el hijo y lo igualamos a `mensaje` que contiene cada post. Con esta modificación ya vuelve a cargarse correctamente los  posts. Con la diferenecia de que ahora en `posts.component.html` usamos el nuevo componente `post` que pinta solo un post que recibe desde el padre usando @input.