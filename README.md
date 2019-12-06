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

```
<h1>App Component</h1>

<app-home></app-home>
<app-about></app-about>
<app-contact></app-contact>
```
<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/appcomponentmod.png">


## Implementar las rutas de nuestra aplicación

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

Con esto me crea el archivo `app-routing.module.ts` el cual debemos abrir. Importaremos las `Routes` que es un tipo manejado por Angular que nos permite implementar las rutas y metetemos una ruta por cada página (componente) que tengamos así como una para cualquier ruta que se meta y no exista que nos lleve a `home`:

```
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
```
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

```
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

```
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
```

Si recargamos la pagina nos redirigue a `http://localhost:4200/home`, si escribimos cualquier cosa como: `http://localhost:4200/gggggssss` nos redirigue a `http://localhost:4200/home`, si escribimos `http://localhost:4200/contact` funciona pero sigue mostrando lo mismo que cuando cargamos la `home`, esto pasa por que en el `app.component.html` le estamos que renderice esto:

```
<h1>App Component</h1>

<app-home></app-home>
<app-about></app-about>
<app-contact></app-contact>
```

En la aplicación no le estamos diciendo donde renderizar estos componentes, para esto vamos a cambiar el código  y usaremos el elemente `router-outlet`, cambiamos el código anterior por este:

```
<h1>App Component</h1>

<router-outlet></router-outlet>
```

Al cargar de nuevo la aplicación, cada componente se cargara cuando se ponga su correspondiente ruta.

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/home.png">
<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/contact.png">
<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/images/about.png">


