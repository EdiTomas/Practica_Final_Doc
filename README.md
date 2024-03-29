# Practica_Final_Doc
# Manual Tecnico  Solvencia

## Tabla de contenidos:
- [Practicante](#practicante)
- [ Solvencia ](#1-solvencia)
    - [Tabla y Campo](#2-tabla-y-campo)
    - [Estudiante perfil](#3-estudiante-perfil)
    - [Administracion](#4-administracion)
    - [Creacion de Archivos](#5-creacion-de-archivos)
    - [Actualizacion de Archivos](#6-actualizacion-de-archivos)
    - [Metodos Proceso de Firmas](#7-metodos-proceso-de-firmas)
    
    
- [Manual técnico](#manual-técnico)

## Practicante:

| Carnet    | Nombre       |
|-----------|--------------|
| 201503783 | Edi Yovani Tomas Reynoso | 

## 1. Solvencia
Diseñar una nueva aplicación de solicitud de solvencia
electrónica del Departamento de Procesamiento de Datos de
la universidad de San Carlos de Guatemala, con la finalidad
de que los estudiantes del Centro Universitario del Sur puedan realizar una solicitud
de solvencia electrónica la cual se utilizará tecnología reciente de software como MySQL y
PHP en su versiones más recientes o actualizadas.


## 2. Tablas y Campos

* **proceso firma:**
    | Atributo | Tipo   | Nulo |
    |----------|--------|-------------|
    | carnet     | decimal(10,0) | No |
    | carrera | int | No |
    | solvencia_tipo | smallint | No |
    | orden_pago | varchar(100) | No |
    | firma_biblioteca | smallint | Si |
    | descripcion_biblioteca | longtext | Si |
    | nota_biblioteca | longtext | Si |
    | firma_lab_basico | smallint | Si |
    | descripcion_lab_basico  | longtext | Si |
    | nota_basico  | longtext | Si |
    | firma_lab_experimental | smallint | Si |
    | descripcion_lab_experimental  | longtext | Si |
    | nota_experimental | longtext | Si |
    | 	firma_bodega | smallint | Si |
    | descripcion_bodega  | longtext | Si |
    | nota_bodega | longtext | Si |
    | 	firma_tesoreria | smallint | Si |
    | descripcion_tesoreria  | longtext | Si |
    | nota_tesoreria | longtext | Si |
    | 	firma_coordinador | smallint | Si |
    | descripcion_coordinador  | longtext | Si |
    
* **historial_solvencia:**

     Atributo | Tipo   | Nulo |
    |----------|--------|-------------|
    | anio     | int | No |
    | correlativo    | int | No |
    | carnet | int | No |
    | carrera | decimal(10,0) | No |
    | data | longtext | No |
    | fecha | datetime | No |
    | usuario | decimal(14,0) | No |
    | actualfirma | varchar(250) | No |


  
* **solicitud_solvencia:**
    | Atributo | Tipo   | Nulo |
    |----------|--------|-------------|
    | id     | int | No |
    | fecha | timestamp | No |
    | carnet | decimal(10,0) | No |
    | carrera | varchar(32) | No |
    | solvencia_tipo | smallint | No |
    | estado | int | No |
    | orden_pago | varchar(32) | No |
    | solvencia_correlativo | int | Si |
    | impresion_fecha  | timestamp | Si |
    | impresion_fecha_adm  | timestamp | Si |
    | impresion_usuario | varchar(128) | Si |
    | impresion_usuario_adm  | varchar(128) | Si |
    | estado_firma | smallint | No |
    | Encargado_Firma_Biblioteca | varchar(200) | Si |
    | Encargado_Firma_Lab_Basico  | varchar(200)  | Si |
    | Encargado_Firma_Lab_Experimental | varchar(200)  | Si |
    | Encargado_Firma_Bodega | varchar(200)  | Si |
    | Encargado_Firma_Tesoreria  | varchar(200)  | Si |
    | Encargado_Firma_Coordinador | varchar(200)  | Si |
    

## 3. Estudiante perfil


**solicitud_solvencia:**
Aqui se inicia la solvencia del estudiante con el cual, podra solicitar y generar una boleta de pago.

![image](/Img/solvencia.png)

En la siguiente se mostrará el codigo que se agrego al perfil estudiantil para generar la solvencia.

***Verificacion de la firma :***
Este apartado consiste en cargar los datos de las solvencias ya firmada por cada unidad a la que corresponde.

```
foreach($SolvenciaPagada as $s){
                     $ProcesoFirmas->getsigning($id,$s["orden_pago"],$solvenciaSolicitudHelper);
}
        
```
![image](/Img/Verficar.PNG)


***Encriptar Datos:***
Esta parte del codigo realiza la encriptacion de los datos de la solvencia.

Se iniciliza las variables para la incriptacion 
```
     $No_Solvencia = 0;  
     $data_encriptar = [];
     $key_security=strtoupper("defecto");
            
```
Se realiza la llamada a los metodo para poder incriptar los datos.


```
    $correlativo = $hSolvenciaTable->getSiguienteCorrelativo(date("Y"));
    $key_security=$Key->Llave_de_Seguridad($carnet,$carrera,$Impresion_fecha_Admin,$correlativo);
    $data_encriptar=$Key->datos_solvencia($carnet,$key_security,$Impresion_fecha_Admin,$carrera,$correlativo,$solvenciaSolicitudHelper);
    $No_Solvencia = $Key->No_Solvencia($correlativo);

```
![image](/Img/EncriptarDatos.PNG)
![image](/Img/EncriptarDatos1.PNG)


***Proceso de Firma:***
Esta parte del codigo se realiza la insercion de los datos previos a la tabla de Proceso de Firmas.

![image](/Img/procesofirma.PNG)



## 4. Administracion

**Inspeccionar Firmas:**
Aqui se verifica las fimara de cada encargado.

**Metodo de Inspeccionar Firmas:**
Este metodo se encuentra en el archivo indexController, nos ayudar a validar las firmas desde la pagina.

![image](/Img/metodoInspeccionar.PNG)

![image](/Img/actualizarFirmas.PNG)


    
    


## 5. Creacion de Archivos
* **Descripcion:**
         Para este practica se creò algunos archivos que se encuentra el carpeta Entities.

* **Listado  de Archivos de la carpeta Entities:**
    | Archivos |  Descripcion   |
    |----------|------------------------------------------------------------------------|
    |LlaveSolvencia.php | Este archivo se creò para encriptar los datos de la solvencia.|
    |HistorialSolvencia.php | Se tomò como base el archivo Historial de Certificaciòn.|
    |ProcesoFirmas.php | Este archivo se creò para realizar inserciones y actualizaciones en el proceso de firma.|
    |SolicitudSolvencia.php | Se tomò como base el archivo de Solicitud de Certificaciòn .|
    |RolTable.php | Este archivo se creò para consultar los Roles de los Usuarios.|

* **Listado  de Archivos de la carpeta Helper:**
   
    | Archivos |  Descripcion   |
    |----------|------------------------------------------------------------------------|
    |SolvenciaHelper.php | Este archivo se creò para crear la solvencia.|

* **Listado  de Archivos en la carpeta Administrativo:**
   
    | Archivos |  Descripcion   |
    |----------|------------------------------------------------------------------------|
    |inspeccionarfirmas.phtml | Este archivo contiene el diseño de la pagina verificar firmas.|
    |solvencia.phtml | Este archivo contiene el diseño de la pagina de solvencia.|

## 6. Actualizacion de Archivos

* **Listado  de Archivos en la carpeta Administrativo y Estudiantil:**

    | Archivos |  Descripcion   |
    |----------|------------------------------------------------------------------------|
    |indexController.php | Este archivo fue modificado para agregarle metodos para el nuevo funcionamiento de la pagina.|



## 7. Metodos Proceso de Firmas
**Funcion add :**
    Metodo que agregar a la tabla proceso de firma.
![image](/Img/add.PNG)


**Funcion getAllConfirmarSolvencia :**
    Este metodo devuelvo un innerjoin de todos los datos del estudiante, que pagaron la solvencia y hace falta de firmar.
![image](/Img/getAllConfirmar.PNG)

**Funcion Lista_SolvenciaPagada :**
    Devuelve el listado de la Solvencia Pagada.
![image](/Img/Lista_solvencia_pagada.PNG)

**Funcion getsigning :**
    Este metodo se firma conforme a a la carrera.
![image](/Img/getsigning.PNG)

**Las funciones firmas :**
    Los Metodos que actualizan  la tabla proceso de firma.
![image](/Img/firma.PNG)




































