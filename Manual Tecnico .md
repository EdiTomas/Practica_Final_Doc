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
    ```
          public function solvenciaAction(){
        
        
        if ($this->permitirIngreso() == true) {
            $id_rubro_solvencia=138;
            $id_rubro_variante_solvencia=1;
            $estudianteHelper = new \DBEntities\Entities\EstudianteHelper($this->adapter, $this->authService);
            $estudianteTable = new \DBEntities\Entities\EstudianteTable($this->adapter);
            $solvenciaSolicitudHelper = new \DBEntities\Entities\SolicitudSolvencia($this->adapter);
            $configuracionTable = new \DBEntities\Entities\ConfiguracionTable($this->adapter);
            $carreraTable = new \DBEntities\Entities\CarreraTable($this->adapter);
            $ProcesoFirmas = new \DBEntities\Entities\ProcesoFirmas($this->adapter);
            //$solvenciaHelper = new \DBEntities\Helper\SolvenciaHelper();
    
            $carrera = (!empty($this->params()->fromPost("carrera"))) ? $this->params()->fromPost("carrera") : "SINCARRERA";
            $id = $this->authService->getIdentity()->getId();
            $carreras = $estudianteHelper->getCarrerasEstudiante(["id" => $id]);
            $estudiante = (!empty($id)) ? $estudianteTable->select(["carnet" => $id])->toArray() : []; //el metodo toArray() devuelve un array de las tuplas
            $ordenPagoHelper = new \DBEntities\Helper\OrdenPagoHelper($this->adapter, $this->authService);
            $dataConfirmar = $data = (!empty($id)) ? $solvenciaSolicitudHelper->getPendientesConfirmarSolvencia($id) : [];
            $SolvenciaPagada = (!empty($id)) ? $solvenciaSolicitudHelper->ConfirmarSolvencia($id) : [];
          
            // $fecha_orden_pago =$this->date("Y-m-d");
          
            foreach ($dataConfirmar as $o) {
                $ordenPagoHelper->confirmarPago($o["orden_pago"], $o["carnet"]);
                 // fecha de orden de pago  
            }
            /*
               Este for verifica la solvencia pagada de cada estudiante correspondiente a su carnet y orden de pago
            */
            foreach($SolvenciaPagada as $s){
                $ProcesoFirmas->getsigning($id,$s["orden_pago"],$solvenciaSolicitudHelper);
                //$this->layout()->setVariable("mensajes", [["error" => true, "mensaje" => $s["orden_pago"]]]);
                     
            }
           

                            

            if ($this->getRequest()->isPost()) {
          
             //  $solvenciaHelper->generarsolvencia($estudiante[0], $carreras["data"], 23,$carrera);
            
              if (!empty($id) && !empty($carrera) && !empty($this->params()->fromPost("tiposolvencia"))) {
                switch ($this->params()->fromPost("tiposolvencia")) {
                      case 1:
                         $this->layout()->setVariable("mensajes", [["error" => true, "mensaje" => "Biblioteca"]]);
                         $tiposolvencia = $this->params()->fromPost("tiposolvencia");
                         $result = $solvenciaSolicitudHelper->solicitudPreviaSolvencia($id, $tiposolvencia);
                         if(!empty($result)){
                         $this->layout()->setVariable("mensajes", [["error" => true, "mensaje" => "Usted aun cuenta con una solicitud sin concluir, por favor, realice el pago o espere a que termine el proceso."]]);
                         }else{
                            //$this->layout()->setVariable("mensajes", [["error" => true, "mensaje" => "Se genero la boleta con exito."]]);
                            //generar la orden de pago
                            $carreraData = $carreraTable->select(["carrera" => $carrera])->toArray()[0];
                            $dataMontos = json_decode($configuracionTable->select(["nombre" => "solvenciaMonto"])->toArray()[0]["contenido"], true); // dudas
                            
                            // Datos a enviar a SW
                            $orden_pago = $ordenPagoHelper->crearOrdenPagoSolvencia($carreraData["ext"], $carreraData["ua"], $carreraData["car"], $id, $carrera, $estudiante[0]["nombre"], $id_rubro_variante_solvencia, $tiposolvencia, $dataMontos[$id_rubro_variante_solvencia]);
                            if ($orden_pago > 0) {
                                //crear solicitud y ademas insertar en la tabla de proceso_firmas
                                $dataprocesofirma=[
                                "carnet" => $id, 
                                "carrera" => $carrera, 
                                "orden_pago" => $orden_pago, 
                                "solvencia_tipo" => $this->params()->fromPost("tiposolvencia"),
                                "firma_biblioteca" =>0,
                                "firma_lab_basico" => 0, 
                                "firma_lab_experimental"=>0,
                                "firma_bodega"=>0,
                                "firma_tesoreria"=>0,
                                "firma_secretaria"=>0,
                                "firma_coordinador"=>0
                                
                                ] ;   
                                $dataInsert = ["carnet" => $id, "carrera" => $carrera, "orden_pago" => $orden_pago, "solvencia_tipo" => $this->params()->fromPost("tiposolvencia"), "estado" => 0,
                                "estado_firma"=>0];
                                $solvenciaSolicitudHelper->insert($dataInsert);
                                $ProcesoFirmas->add($dataprocesofirma);
                                $this->layout()->setVariable("mensajes", [["error" => false, "mensaje" => "Su solicitud se ha registrado con éxito, por favor, realice el pago de la orden de pago para continuar el proceso."]]);
                            } else {
                                $this->layout()->setVariable("mensajes", [["error" => true, "mensaje" => "Hubo un problema al intentar generar la orden de pago, intente de nuevo más tarde."]]);
                            }
                        }           
                      break;

                }
                      
              } else if ($id == $this->params()->fromPost("orden_pago_carnet") && !empty($this->params()->fromPost("orden_pago_carrera")) && !empty($this->params()->fromPost("orden_pago"))) {
                $orden_pago = $this->params()->fromPost("orden_pago");
                $dataOrden = $solvenciaSolicitudHelper->getDataOrdenSolvencia($orden_pago);
                $solvenciaSolicitudHelper->actualizarFechaImpresion($orden_pago, $id);
                // **********Linea 248
                if($dataOrden[0]["solvencia_tipo"] == "1"){
                    $ordenPagoHelper->imprimirOrdenPago($dataOrden, "Pago de Solvencia  de Biblioteca");
                } 
                  
              }else if($id == $this->params()->fromPost("solv_carnet") && !empty($this->params()->fromPost("solv_carrera")) && !empty($this->params()->fromPost("solv_orden"))){
               
                if ($this->params()->fromPost("solvencia_tipo") == 1) {
                    
                    $carnet = $this->params()->fromPost("solv_carnet");
                    $carrera = $this->params()->fromPost("solv_carrera");
                    $name_career = $this->params()->fromPost("solv_carrera_nombre");   
                    $orden_pago = $this->params()->fromPost("solv_orden");
                    $Impresion_fecha_Admin = $this->params()->fromPost("impresion_fecha_adm");
                    
                        
                        $hSolvenciaTable = new \DBEntities\Entities\HistorialSolvencia($this->adapter);
                         /* 
                          Se realiza una instanca de la clase LlaveSolvencia,
                          para incriptar la informacion personal del estudiante. 
                        */
                         $Key = new \DBEntities\Entities\LlaveSolvencia($this->adapter);
                        
                        // Verificar si ya se le asoció un correlativo para no duplicar correlativos en la impresión... 
                        $resultadoYaImpresa = $solvenciaSolicitudHelper->getSolvenciaYaImpresa($orden_pago);
                        /*
                          Se inicializan las variables.
                        */
                        $No_Solvencia = 0;  
                        $data_encriptar = [];
                        $key_security=strtoupper("defecto");
                        
                        if (empty($resultadoYaImpresa)) {
                            
                            $correlativo = $hSolvenciaTable->getSiguienteCorrelativo(date("Y"));
                            $key_security=$Key->Llave_de_Seguridad($carnet,$carrera,$Impresion_fecha_Admin,$correlativo);
                            $data_encriptar=$Key->datos_solvencia($carnet,$key_security,$Impresion_fecha_Admin,$carrera,$correlativo,$solvenciaSolicitudHelper);
                            $No_Solvencia = $Key->No_Solvencia($correlativo);
                            //actualizar el estado del documento 
                            $solvenciaSolicitudHelper->actualizarFechaImpresionAdm($orden_pago, $this->authService->getIdentity()->getId(), $correlativo);
                            //generar el documento
                            $hSolvenciaTable->insertar($carnet, $carrera, $correlativo, $this->authService->getIdentity()->getId(),$data_encriptar);
                        } else {
                           
                            $correlativo = $hSolvenciaTable->getSiguienteCorrelativo(date("Y"));
                            $key_security=$Key->Llave_de_Seguridad($carnet,$carrera,$Impresion_fecha_Admin,$correlativo);
                            $data_encriptar=$Key->datos_solvencia($carnet,$key_security,$Impresion_fecha_Admin,$carrera,$correlativo,$solvenciaSolicitudHelper);
                            $No_Solvencia = $Key->No_Solvencia($correlativo);
                        
                            $solvenciaSolicitudHelper->actualizarFechaImpresionAdm($orden_pago, $this->authService->getIdentity()->getId(), $correlativo);
                            $hSolvenciaTable->insertar($carnet, $carrera, $correlativo, $this->authService->getIdentity()->getId(),$data_encriptar);
                      
                        }
                        $solvenciaHelper = new \DBEntities\Helper\SolvenciaHelper();
                        $estudiante = $estudianteTable->select(["carnet" => $id])->toArray();
                        return $solvenciaHelper->GenerarSolvenciaPrueba($estudiante[0], $name_career, $No_Solvencia,$carrera,$key_security);
                }
                $solvenciaSolicitudHelper->actualizarFechaImpresionAdm($orden_pago, $this->authService->getIdentity()->getId(), null);
                $this->layout()->setVariable("mensajes", [["error" => false, "mensaje" => "Impresión marcada con éxito."]]);
               
              }
              
                
            }
                 
            $data = (!empty($id)) ? $solvenciaSolicitudHelper->get($id) : [];
            return new ViewModel([
                "data"=>$data,
                "carreras" => (!empty($carreras)) ? $carreras["data"] : [],
                "dataEstudiante" => (!empty($estudiante)) ? $estudiante[0] : NULL ,//verifico si encontro un estudiante, si lo encontro devuelvo el primero
            ]);
        }       
        
        
    }


    ```






























