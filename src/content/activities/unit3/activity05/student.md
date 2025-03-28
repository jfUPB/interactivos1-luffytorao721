# Modelo Gráfico de la Bomba Temporizada

## Diagrama de Estados 

```mermaid
stateDiagram-v2
    [*] --> CONFIGURACION
    CONFIGURACION --> ARMADA: Evento Shake ('S')
    ARMADA --> CONFIGURACION: Secuencia A-B-A-S
    ARMADA --> EXPLOSION: Tiempo = 0
    EXPLOSION --> CONFIGURACION: Reset automático
    
    state CONFIGURACION {
        [*] --> AJUSTE_TIEMPO
        AJUSTE_TIEMPO: Botones A/B ajustan tiempo (10-60s)
    }
    
    state ARMADA {
        [*] --> CUENTA_REGRESIVA
        CUENTE_REGRESIVA: Muestra tiempo restante
        CUENTA_REGRESIVA --> SECUENCIA: Eventos A/B/S/T
    }
    
    state EXPLOSION {
        [*] --> ANIMACION
        ANIMACION: Sonido + Imagen SKULL
    }

