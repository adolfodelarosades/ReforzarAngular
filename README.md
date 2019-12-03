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

`ng g c pages/about`

Dentro de **app/pages/about** se crean todos los archivos del componente, así como también se modifica el archivo **src/app/app.module.ts** para incluir la referencia al nuevo componente.

### Crear el componente contact

`ng g c pages/contact`

Pasa lo mismo que al crear el componente about.

### Componente app.component

El componente `app.component` es el componente inicial que se se carga cuando ejecutamos nuestra aplicación:

<img src="https://github.com/adolfodelarosades/ReforzarAngular/blob/master/appcomponent.png">





Como es exactamente lo que queremos hacer ejecutamos el comando sin esos parámetros, es decir:


