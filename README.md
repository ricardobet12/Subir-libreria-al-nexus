# Subir-libreria-al-nexus
Configuración de librería en angular para subir archivo .tgz


## ng-packagr 
es una biblioteca que permite compilar y empaquetar una biblioteca de TypeScript en formato de paquete angular.
lo usaremos para sacar lo componentes de nuestro proyecto de CLI y empaquetarlos en un formato que pueda usarse dentro de 
otras aplicaciones angular.

 ## paso 1.

en la terminal y ubicados en la raiz de nuestro proyecto ejecutamos el comando " npm install -D ng-packagr"  esto descargara
ng-packagr y lo declarara como parte de devDependency de nuestro proyecto en el  archivo package.json.

segun ng-packagr, tendremos que agregar dos archivos nuevos, estos archivos se crearan en la raiz de nuestro proyecto,
"ng-package.json" y "public_api.ts".

## paso 2

en la raiz de nuestro proyecto creamos un archivo llamado "ng-package.json" y dentro de el agregamos lo siguiente:

`
{ 
  "$ schema": "./node_modules/ng-packagr/ng-package.schema.json", 
  "lib": { 
    "entryFile": "public_api.ts" 
  } 
}
`

luego de nuevo en la raiz de nuestro proyecto creamos un archivo llamado "public_api.ts"  en donde exportamos los componentes y archivos
que seran empaquetados y que se usaran en otras aplicaciones angular.

para nuestro ejemplo exportaremos en app.module.

export * from './src/app/app.module';

## paso 3.


Ahora agregaremos un package script a nuestro package.json para indicar que  ng-packagr vamos a usar, en este caso usaremos en 
ng-package.json que creamos anteriormente. ademas cambiamos el atributo private a false para que asi pueda publicar su biblioteca cuando lo nesecite.

`
"scripts": { 
  "ng": "ng", 
  "start": "ng serve", 
  "build": "ng build", 
  "test": "ng test", 
  "lint": "ng lint", 
  " e2e ":" ng e2e ", 
  " packagr ":" ng-packagr -p ng-package.json " 
}, 
" private ": false

...
`


## CONFIGURACION DE NEXUS CON NPM

## paso 4.

en el archivo "package.json" agregarmos lo siguiente:
`
{
  ... ,
  "publishConfig": {
    "registry": "http://nexus.softwareosr.com/repository/npm-private/"
  }
}
`
esto con el fin de que nos permita subir  nuestros proyectos en el repositorio de nexus.

## paso 5.

en la raiz de nuestro proyecto creamos un archivo con el siguiente nombre ".npmrc" y pondremos lo siguiente:

registry=http://nexus.softwareosr.com/repository/npm-private/



## paso 6.


En la terminal ejecutamos el siguiente comando:

echo -n 'myuser:@user' | openssl base64

en donde "@user" es el nombre de usuario de nexus.
esto nos devolvera un hash en base64 para las credenciales con repositorio de nexus, para que la conexion sea efectiva.


## paso 7.

volviendo a nuetro archivo ".npmrc" y agregamos:

_auth=hash

en donde "hash" es el valor que nos retorno en el paso anterior.


## paso 8.

en la terminal y ubicados en la raiz de nuestro proyecto ejecutamos el comando "npm addUser", debera ingresar el nombre de usuario, clave y correo correspondientes 
a su cuenta de nexus.


## CREANDO NUESTRO PAQUETE

luego de realizar los pasos anteriores ejecutamos en nuestra terminal "ng build", una vez que el proceso se haya completado, encontraremos una caperta llamada "dist" en la raiz 
de nuestro proyecto. Esta es nuestra blibioteca de componentes.



## PUBLICAR BIBLIOTECAS EN NEXUS

ya con los pasos anteriores terminados ejecutamos en la terminal el siguiente comando.

npm publish --registry http://nexus.softwareosr.com/repository/npm-private/
npm config set registry https://registry.npmjs.com/

y ya con esto deberia estar publicado nuestra biblioteca en nexus.
