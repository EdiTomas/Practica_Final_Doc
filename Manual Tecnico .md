# Practica_Final_Doc
# Manual Tecnico  Solvencia

## Tabla de contenidos:
- [Practicante](#practicante)
- [ Solvencia ](#1-solvencia)
    - [Tabla y Campo](#2-tabla-y-campo)
    - [Estudiante perfil](#3-estudiante-perfil)
    - [Administracion](#4-administracion)
    - [Eliminación de usuario](#5-eliminación-de-usuario)
    - [Creación de Evento Electoral](#6-creación-de-evento-electoral)
    - [Consulta de Evento Electoral](#7-consulta-de-evento-electoral)
    - [Actualización de Evento Electoral](#8-actualización-de-evento-electoral)
    - [Eliminación de Evento Electoral](#9-eliminación-de-evento-electoral)
    - [Registro de planilla](#10-registro-de-planilla)
    - [Eliminación de planilla](#11-eliminación-de-planilla)
    - [Consulta de Planillas](#12-consulta-de-planillas)
    - [Consulta de Candidatos](#13-consulta-de-candidatos)
    - [Aprobación de planillas](#14-aprobación-de-planillas)
    - [Votación](#15-votación)
    - [Consulta de Resultados](#16-consulta-de-resultados)
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
    

## 2. Estudiante perfil


**solicitud_solvencia:**
Aqui se inicia la solvencia del estudiante con el cual, podra solicitar y generar una boleta de pago.

En la siguiente se mostrará el codigo que se agrego al perfil estudiantil para generar la solvencia.

![image](/Img/solvencia.png)

***Verificacion de la firma :***
Este apartado consiste en cargar los datos de las solvencias ya firmada por cada unidad a la que corresponde.

```
         /*

                foreach($SolvenciaPagada as $s){
                     $ProcesoFirmas->getsigning($id,$s["orden_pago"],$solvenciaSolicitudHelper);
                }
                     
        */
```


***Encriptar Datos:***

Se iniciliza las variables para la incriptacion de codigo
    ```
          {

                        $No_Solvencia = 0;  
                        $data_encriptar = [];
                        $key_security=strtoupper("defecto");
            }


    ```
Esta parte del codigo realiza la encriptacion de los datos de la solvencia 
     ```
          {

                            $correlativo = $hSolvenciaTable->getSiguienteCorrelativo(date("Y"));
                            $key_security=$Key->Llave_de_Seguridad($carnet,$carrera,$Impresion_fecha_Admin,$correlativo);
                            $data_encriptar=$Key->datos_solvencia($carnet,$key_security,$Impresion_fecha_Admin,$carrera,$correlativo,$solvenciaSolicitudHelper);
                            $No_Solvencia = $Key->No_Solvencia($correlativo);
                          
            }


    ```






























