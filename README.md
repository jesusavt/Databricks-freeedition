# 🛡️ Sistema de Detección de Fraude Transaccional (Medallion Architecture)

Este repositorio contiene un pipeline de datos de extremo a extremo desarrollado en **Databricks** para la identificación y análisis de fraudes en transacciones bancarias. El proyecto utiliza una arquitectura **Medallion** para garantizar la calidad, trazabilidad y gobernanza de los datos en un entorno de **Unity Catalog**.

## 🚀 Descripción del Proyecto
El sistema procesa datos históricos de transacciones para identificar patrones de fraude. Utiliza **Spark (PySpark)** para la limpieza y enriquecimiento de datos, y **Delta Lake** para el almacenamiento incremental.

### Características principales:
- **Modelo Estrella:** Optimizado para analítica y reportes de alto rendimiento.
- **Detección de Riesgo:** Cálculo de lógica de "Hora Pico" y "Tasa de Fraude".
- **Gobernanza:** Estructura clara de capas Bronze, Silver y Gold.
- **Automatización:** Pipeline integrable con GitHub Actions.

---

## 🏗️ Arquitectura de Datos
El flujo de datos se divide en tres etapas de refinamiento:
1. **Capa Bronze:** Ingesta de archivos crudos (CSV) con preservación de historia.
2. **Capa Silver:** Limpieza de nulos, deduplicación y casteo de tipos de datos.
3. **Capa Gold:** Generación de dimensiones y tabla de hechos enriquecida.

---

## 📊 Diagrama Entidad-Relación (ERD)
El modelo en la capa **Gold** sigue un esquema en estrella diseñado para responder preguntas de negocio rápidamente:

```mermaid
erDiagram
    FACT_TRANSACCIONES ||--o{ DIM_CLIENTES : "realizada_por"
    FACT_TRANSACCIONES ||--o{ DIM_TARJETAS : "usando"
    FACT_TRANSACCIONES ||--o{ DIM_MCC : "ocurre_en"

    FACT_TRANSACCIONES {
        string id_transaccion_sk PK
        timestamp fecha_completa
        long id_cliente FK
        long id_tarjeta FK
        long codigo_mcc FK
        double monto
        long es_fraude
        boolean hora_pico
        string dia_semana
        string process_date
    }

    DIM_CLIENTES {
        long id_cliente PK
        string nombre
        string rango_edad
        string rango_fico
        string estado
        double ingreso_anual
    }

    DIM_TARJETAS {
        long id_tarjeta PK
        string tipo_tarjeta
        string marca_tarjeta
        double limite_credito
    }

    DIM_MCC {
        long codigo_mcc PK
        string descripcion
        string categoria_grupo
    }
