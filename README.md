##Diagrama de Clases

```mermaid
classDiagram
    direction BT

    class SmartGrids {
        - plantas: Map~String, Planta~
        + registrarPlanta(Planta p)
        + cambiarEstado(String nombre)
        + registrarMonitoreo(...)
        + calcularHistograma(...)
        + rankingEnergia()
        + limpiarRegistrosVacios()
    }

    class Planta {
        # String nombre
        # String direccion
        # double capacidadMaxima
        # boolean activa
        # LocalDateTime fechaPuestaEnMarcha
        # List~PeriodoInactividad~ inactividades
        # List~RegistroMonitoreo~ registrosMonitoreo
        + abstract boolean cumpleCuota(...)
        + agregarRegistro(RegistroMonitoreo r)
        + double calcularTotalGenerado()
        + int limpiarRegistrosSinDatos()
    }

    class PlantaRenovable {
        # String fuentePrimaria
        # double eficienciaPromedio
    }

    class PlantaSolar {
        - double superficieCubierta
        - int cantidadPaneles
        - double horasRadiacionDiaria
        + boolean cumpleCuota(...)
    }

    class PlantaEolica {
        - int cantidadAerogeneradores
        - double alturaTorres
        - double velocidadMinimaViento
        + boolean cumpleCuota(...)
    }

    class PlantaHidraulica {
        - int cantidadTurbinas
        - double capacidadEmbalse
        + boolean cumpleCuota(...)
    }

    class PlantaTermica {
        - String tipoCombustible
        - double objetivoEmisionesCO2
        - double capacidadAlmacenamientoCombustible
        + boolean cumpleCuota(...)
    }

    class RegistroMonitoreo {
        - LocalDateTime fechaHora
        - double energiaGenerada
        - int incidencias
    }

    class RegistroMonitoreoTermico {
        - double co2Generado
        - double combustibleUtilizado
    }

    class RegistroMonitoreoEolico {
        - double velocidadVientoPromedio
    }

    class PeriodoInactividad {
        - LocalDateTime inicio
        - LocalDateTime fin
    }

    %% Relaciones de Herencia (Generalización)
    Planta <|-- PlantaRenovable
    PlantaRenovable <|-- PlantaSolar
    PlantaRenovable <|-- PlantaEolica
    PlantaRenovable <|-- PlantaHidraulica
    Planta <|-- PlantaTermica

    RegistroMonitoreo <|-- RegistroMonitoreoTermico
    RegistroMonitoreo <|-- RegistroMonitoreoEolico

    %% Relaciones de Asociación
    SmartGrids "1" --> "*" Planta : administra
    Planta "1" --> "*" PeriodoInactividad : registra inactividades
    Planta "1" --> "*" RegistroMonitoreo : almacena sus registros
