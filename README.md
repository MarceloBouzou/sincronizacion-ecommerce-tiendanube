# Caso de Estudio: Sincronización de Inventario para E-commerce (TiendaNube)

## (Resumen del Proyecto)

Este repositorio documenta una solución de automatización que **sincroniza en tiempo real el inventario (precios y stock) entre una Google Sheet y una tienda de E-commerce en la plataforma TiendaNube**. El sistema fue diseñado para ser operado por un usuario no-técnico y, crucialmente, para replicar la funcionalidad de carga masiva de un plan premium **sin incurrir en el costo de suscripción**, utilizando directamente la API de TiendaNube.

---

## 🎯 El Desafío: Habilitar un Nuevo Negocio con Cero Fricción y Mínima Inversión

Un socio de negocio necesitaba lanzar una tienda online para la venta de carne, pero enfrentaba tres desafíos críticos:

1.  **Operador No-Técnico:** La persona encargada de actualizar precios y stock manejaba únicamente una Google Sheet simple y no tenía conocimientos sobre SKUs, IDs de producto o cómo operar el backend de una tienda online.
2.  **Restricción de Costos:** El negocio no deseaba (o no podía) pagar por el plan premium de TiendaNube, que era el único que permitía la actualización masiva de productos a través de un archivo Excel.
3.  **Necesidad de Automatización Total:** El objetivo era que el proceso fuera completamente desatendido. La actualización en la Google Sheet debía reflejarse en la tienda "mágicamente", sin intervención manual.

---

## 🛠️ Mi Solución: Una Arquitectura de Sincronización Asíncrona

Diseñé y construí un sistema de sincronización totalmente automático que funciona como un puente invisible entre la simplicidad de la Google Sheet del usuario y la complejidad de la API de TiendaNube.

### Arquitectura y Flujo de Trabajo:

1.  **Capa de "Traducción" (Sheet Puente):** Creé una Google Sheet intermedia que actúa como el cerebro del sistema. Esta "Sheet Puente" se conecta a la API de TiendaNube para obtener los `product_id` y `variant_id` de todos los artículos de la tienda.
2.  **Mapeo Inteligente:** La Sheet Puente utiliza fórmulas para "traducir" las descripciones en lenguaje humano de la planilla del usuario (ej: "Lomo Frigo1") al ID de variante exacto que la API de TiendaNube necesita para una actualización.
3.  **Disparador de Eventos (Trigger `onEdit`):** Un script en Google Apps Script se ejecuta en la planilla del usuario. Para evitar sobrecargar la API, el script espera 10 minutos después de la última edición antes de copiar los datos actualizados a la Sheet Puente.
4.  **Sincronización Programada (Trigger de Tiempo):** Cada dos horas, otro script se ejecuta automáticamente en la Sheet Puente. Este script recorre los productos actualizados, construye las peticiones `PUT` necesarias y envía los nuevos precios y stock a la API de TiendaNube, actualizando la tienda de forma masiva y desatendida.

---

## 🚀 Impacto Cuantificable en el Negocio

*   **Habilitación de una Nueva Unidad de Negocio:** La solución permitió el lanzamiento y la operación de una tienda online que de otra manera no habría sido viable por las barreras técnicas y de costos.
*   **Ahorro de Costos Directo y Permanente:** Se **evitó un costo de suscripción recurrente** al replicar la funcionalidad del plan premium a través de la API, generando un ROI inmediato y perpetuo.
*   **Eliminación del 100% del Trabajo Manual:** El proceso es completamente autónomo, eliminando cualquier necesidad de intervención manual para la actualización de inventario.
*   **Cero Fricción para el Usuario:** El sistema permitió que un operador no-técnico gestionara el inventario de una tienda online compleja utilizando únicamente la herramienta que ya conocía (una Google Sheet simple).

---

## 💻 Stack Tecnológico

*   **Plataforma Principal:** Google Apps Script (JavaScript)
*   **Base de Datos / Interfaz de Usuario:** Google Sheets
*   **Integración Principal:** TiendaNube REST API
*   **Arquitectura:** Basada en Eventos (Triggers `onEdit` y de Tiempo)
