# Modelo de Dominio - Sistema de Difusión Sindical

## 1. Reglas de Negocio y Restricciones

- **Límite de Suscripción:** Un afiliado puede estar suscrito a un máximo de dos (2) canales de forma concurrente.
- **Restricción de Dispositivos Asociados:** Un afiliado puede tener asociado, como máximo, un (1) dispositivo móvil activo.
- **Difusión de Comunicados:** Un `Comunicado` puede publicarse a través de múltiples canales, o bien dirigirse de manera directa y personalizada a un conjunto específico de afiliados.
- **Límite de Adjuntos:** Un `Comunicado` puede contener como máximo una (1) imagen adjunta.

---

## 2. Diagrama

```mermaid
classDiagram
    class Administrador {
        correoElectronico
    }

    class Afiliado {
        nif
        nombre
        telefono
        correoElectronico
        estado
    }

    class Comunicado {
        titulo
        cuerpo
        fechaHoraPublicacion
    }

    class Imagen {
        url
    }

    class Canal {
        nombre
    }

    class SuscritoA {
        fechaSuscripcion
    }

    class DispositivoMovil {
        fechaHoraVinculacion
    }

    Administrador "1" --> "0..*" Comunicado : Redacta
    Comunicado "1" *-- "0..1" Imagen : Contiene
    Comunicado "0..*" --> "0..*" Afiliado : DirigidoA
    Comunicado "0..*" --> "0..*" Canal : PublicadoEn
    Afiliado "0..*" -- "0..2" Canal : SuscritoA
    SuscritoA ..> Afiliado : vincula
    SuscritoA ..> Canal : vincula
    Afiliado "1" --> "0..1" DispositivoMovil : TieneAsociado
