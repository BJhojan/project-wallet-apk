# Mi Wallet

[![Python 3.11+](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://www.python.org/)
[![Flet 0.86.5](https://img.shields.io/badge/Flet-0.86.5-green.svg)](https://flet.dev/)
[![License: MIT + Commons Clause](https://img.shields.io/badge/License-MIT%2BCommons%20Clause-orange.svg)](LICENSE)
[![Status: Release package](https://img.shields.io/badge/Status-Release%20package-blue.svg)]()

Aplicación personal de finanzas desarrollada con **Python y Flet** para gestionar cuentas, movimientos, transferencias, deudas y balances desde una interfaz visual intuitiva.

Este repositorio contiene la **versión distribuible pública** de la app junto con capturas de pantalla y el paquete de descarga. **No incluye el código fuente completo del proyecto** ni el entorno de desarrollo, sino la entrega preparada para instalar y probar.

**Objetivo:** Concentrar en una única cartera la información financiera del usuario y ofrecer una consulta rápida del estado actual, historial, deuda y saldo para facilitar la gestión diaria.

---

## 📋 Tabla de contenidos

- [Vista previa](#-vista-previa)
- [Funcionalidades](#-funcionalidades)
- [Instalación](#-instalación-y-configuración)
- [Uso](#-flujo-de-uso)
- [Arquitectura](#-arquitectura)
- [Tecnologías](#-tecnologías)
- [Estado del proyecto](#-estado-del-proyecto)
- [Roadmap](#-roadmap)
- [FAQ](#-faq)
- [Licencia](#-licencia)
- [Contribuciones](#-contribuciones-y-soporte)

---

## 📱 Vista previa

Las capturas reales de la aplicación se encuentran en la carpeta [`imagenes/`](imagenes/).

### Pantalla de inicio
![Pantalla principal](imagenes/Pantalla%20principal.png)

### Resumen de movimientos
![Resumen de movimientos](imagenes/Resumen%20de%20movimientos.png)

### Nuevo gasto
![Nuevo gasto](imagenes/Nuevo%20gasto.png)

### Nueva transferencia
![Nueva transferencia](imagenes/Nueva%20transferencia.png)

### Nueva cuenta
![Nueva cuenta](imagenes/Nueva%20cuenta.png)

### Configuración
![Configuración](imagenes/Configuracion.png)

---

## ✨ Funcionalidades

### 💰 Cuentas y balance

- Crear, editar y eliminar cuentas
- Clasificar cuentas mediante tipos configurables (efectivo, banco, deudas, inversiones, ahorros)
- Crear nuevos tipos de cuenta personalizados
- Consultar saldo individual y balance global de la wallet
- Generar automáticamente movimientos de ajuste cuando sea necesario

### 📊 Movimientos

Cada ingreso o gasto incluye:
- Cuenta, tipo, categoría, descripción, importe y fecha

**Filtros disponibles:**
- Por cuenta
- Por tipo
- Por categoría
- Por rango de fechas

**Consulta por periodos:** Evita cargar innecesariamente todo el historial. La vista de inicio muestra información reciente, mientras que la sección de registros permite consultar periodos más amplios.

### 🔄 Transferencias

Una transferencia relaciona una cuenta de origen con una cuenta de destino mediante un identificador compartido.

- Se presenta como **una única tarjeta de transferencia** en la interfaz
- Internamente afecta al saldo de dos cuentas
- Consultables desde registros con filtros correspondientes

### 💳 Deudas

Una deuda es una **cuenta** (subclase de `Account`, tipo "Deuda"): puede recibir gastos, ingresos y transferencias igual que cualquier cuenta.

**Modelo:**
- Crear, editar y eliminar deudas con nombre, institución, capital, interés, plazo, cuota y fecha del primer pago
- Cada cargo se agrupa por categoría en un **`category_debt`** (categoría, monto, pagado, fecha)
- El balance de la deuda es `pagado − total adeudado` (negativo mientras se deba, 0 al saldarse)
- Se marca **cerrada** cuando lo pagado cubre el total adeudado (capital + intereses)

**Reglas de los movimientos sobre la deuda:**
- **Gasto con la deuda** → aumenta el capital y crea/aumenta el `category_debt` de esa categoría
- **Categoría "Intereses"** → no aumenta el capital, solo el interés acumulado
- **Transferencia saliente** (desde la deuda) → avance/crédito, equivale a un gasto
- **Transferencia entrante / ingreso** (hacia la deuda) → pago que amortiza los `category_debt` del más antiguo al más nuevo, cerrándolos al saldarse
- El pago se imputa **primero a intereses y luego a capital**, registrando cuánto interés se pagó en cada categoría

**Interfaz:**
- Las deudas aparecen como cuentas en Inicio y en los selectores de movimientos/transferencias
- **Convertir cuenta en deuda** desde el appbar de Deudas: el historial de la cuenta se reprocesa con las reglas anteriores
- Calcular automáticamente la próxima fecha de pago mensual
- Registrar pagos desde una cuenta normal asociados a una deuda concreta
- Consultar el detalle con desglose por categorías (capital e interés pagado al cerrarse) y movimientos asociados
- Filtrar entre deudas activas y deudas cerradas
- Ordenar por próximo pago, nombre, saldo pendiente o progreso
- Pagos en verde; gastos, intereses y avances en rojo

### 📈 Predicciones y análisis

La aplicación incorpora predicciones basadas en datos registrados para una referencia sobre la evolución financiera esperada.

> ⚠️ **Estado:** Disponible como código heredado, pero no forma parte de la pantalla principal actual. Se reservará para una futura ventana de análisis.

### ⚙️ Configuración

Personaliza distintos aspectos de la wallet:
- Privacidad (ocultar/mostrar saldos)
- Preferencias de gráficos
- Categorías y tipos de cuenta
- Opciones generales de la aplicación

### 📥 Importación y exportación

**Importación:** Soporta archivos CSV en formato **Money Manager**.

El archivo debe contener las siguientes columnas separadas por comas:

```
account	category, currency, amount, ref_currency_amount, type, payment_type, payment_type_local, note, date, gps_latitude, gps_longitude, gps_accuracy_in_meters, warranty_in_month, transfer, payee, labels, envelope_id, custom_category

```

**Descripción de campos:**

| Campo | Descripción |
|---|---|
| `account` | Nombre de la cuenta |
| `category` | Categoría del movimiento |
| `currency` | Código de moneda (ej: USD, EUR) |
| `amount` | Cantidad en moneda local |
| `ref_currency_amount` | Cantidad en moneda de referencia |
| `type` | Tipo de transacción (ingreso, gasto, etc.) |
| `payment_type` | Método de pago |
| `payment_type_local` | Método de pago en idioma local |
| `note` | Notas o descripción |
| `date` | Fecha del movimiento |
| `gps_latitude` | Latitud (opcional) |
| `gps_longitude` | Longitud (opcional) |
| `gps_accuracy_in_meters` | Precisión GPS (opcional) |
| `warranty_in_month` | Garantía en meses (opcional) |
| `transfer` | ID de transferencia relacionada (si aplica) |
| `payee` | Beneficiario/Pagador |
| `labels` | Etiquetas asociadas |
| `envelope_id` | ID de sobre/presupuesto |
| `custom_category` | Categoría personalizada |

**Exportación:** Crea copias de respaldo o trabaja con datos fuera de la aplicación.

**Persistencia local:** Los datos se conservan tras actualizar o reinstalar la aplicación en Android (siempre que se conserve el almacenamiento de datos).

---

## 🎯 Flujo de uso

1. Crear o seleccionar cuentas financieras
2. Registrar ingresos, gastos o transferencias
3. Crear deudas o convertir una cuenta existente en deuda
4. Registrar gastos y avances directamente sobre la deuda (se agrupan por categoría)
5. Registrar pagos hacia la deuda (se amortizan primero intereses, luego capital, del cargo más antiguo al más reciente)
6. Consultar balances y registros mediante filtros
7. Revisar información de cuentas, deudas y movimientos recientes en Inicio
8. Personalizar desde Configuración
9. Los cambios se guardan automáticamente en almacenamiento local
10. Importar o exportar información cuando sea necesario

---

## 🏗️ Arquitectura

El proyecto separa la interfaz, lógica de negocio, dominio y persistencia, y en esta entrega pública se mantiene la estructura de distribución final:

```
.
├── download.zip       # Paquete de distribución con la versión disponible
├── imagenes/          # Capturas de pantalla de la aplicación
├── LICENSE            # Licencia MIT + Commons Clause
└── README.md          # Documentación del repositorio
```

**Componentes principales de la entrega pública:**
- **Aplicación distributiva:** APK empacado en [`download.zip`](download.zip)
- **Capturas visuales:** Material gráfico para revisión de la interfaz
- **Documentación:** Instrucciones, estado del proyecto y licencia
- **Persistencia local:** La app guarda datos en el dispositivo para uso personal

> Este repositorio no incluye el proyecto completo de desarrollo ni su estructura interna original, sino la versión preparada para uso y distribución pública.

---

## 🔨 Instalación y configuración

### Requisitos previos

- **Android** para instalar la app distribuida
- **Windows, macOS o Linux** para extraer el archivo ZIP
- **Permisos de instalación de fuentes desconocidas** en Android

### Pasos de instalación

#### 1. Descargar la versión disponible

Descarga el archivo [`download.zip`](download.zip) desde este repositorio.

#### 2. Extraer el paquete

Descomprime el archivo ZIP en tu equipo o dispositivo.

#### 3. Instalar el APK

1. Localiza el archivo APK incluido en el ZIP.
2. Transfiérelo al dispositivo Android si es necesario.
3. Abre el APK y acepta la autorización del sistema.

> Android puede mostrar una advertencia al instalar una app fuera de Google Play. Instálala solo si confías en la fuente.
---

## 🛠️ Tecnologías

| Tecnología | Versión | Propósito |
|---|---|---|
| **Python** | 3.11.3 | Lenguaje principal |
| **Flet** | 0.86.5 | Framework UI multiplataforma |
| **Flet Charts** | 0.86.5 | Gráficas y visualizaciones |
| **JSON** | — | Persistencia local de datos |
| **CSV** | — | Importación/Exportación |
| **Pytest** | — | Testing y automatización |

---

## 📲 Descargar

La aplicación puede descargarse desde [`download.zip`](download.zip) en este repositorio.

**Plataformas:**
- ✅ **Android:** Versión optimizada y completa (recomendada)
- 🟡 **Escritorio:** Funcional pero con ajustes visuales pendientes
- 🟡 **Web:** Compatible pero no optimizada

---

## 🔧 Compatibilidad

- **Diseño principal:** Android
- **Base de código:** Multiplataforma mediante Flet
- **Pruebas específicas:** Resueltas diferencias en interacción, selección de archivos y persistencia entre plataformas

**Notas de compatibilidad:**
- El almacenamiento local en Android persiste incluso tras actualizaciones (si se mantiene el almacenamiento de datos)
- En escritorio, los datos se guardan en directorios estándar del sistema

---

## 📋 Estado del proyecto

**Estado actual de la entrega pública:**
- ✅ APK disponible para descarga en [`download.zip`](download.zip)
- ✅ Capturas de pantalla actualizadas en [`imagenes/`](imagenes/)
- ✅ Funcionalidades principales de cartera, movimientos, transferencias y deudas
- ✅ Persistencia local, configuraciones y exportación/importación CSV
- ✅ Documentación y licencia actualizadas
- ❌ No incluye el código fuente del proyecto principal
- ❌ No incluye entorno de desarrollo ni estructura de trabajo completa

**Características implementadas:**
- ✅ Gestión de cuentas (crear, editar, eliminar)
- ✅ Registros de movimientos (ingresos y gastos)
- ✅ Transferencias entre cuentas
- ✅ Balances y consultas
- ✅ Filtros por período, cuenta, tipo y categoría
- ✅ Predicciones iniciales (reservadas para una futura vista de análisis)
- ✅ Gestión completa de deudas como cuentas: gastos, avances e intereses sobre la deuda
- ✅ Cargos de deuda agrupados por categoría (`category_debt`) con imputación de intereses
- ✅ Conversión de una cuenta existente en deuda (reprocesa su historial)
- ✅ Pagos de deuda asociados a cuenta, categoría y `debt_id`
- ✅ Clasificación de deudas activas y cerradas
- ✅ Detalle de deuda con desglose por categoría (capital e interés pagado) y movimientos asociados
- ✅ Deudas visibles en Inicio y seleccionables en movimientos/transferencias
- ✅ Configuración personalizable
- ✅ Persistencia local (JSON)
- ✅ Importación/Exportación CSV (formato Money Manager)
- ✅ Suite de pruebas automatizadas
- ✅ Interfaz responsiva y optimizada para Android

**Evolución continua:** El proyecto evoluciona a partir de pruebas de uso real, enfocándose en ampliar análisis financiero, funcionalidades para deudas e inversiones, y cálculos automáticos.

---

## 🚀 Roadmap

| Fase | Estado | Descripción |
|---|---|---|
| **1 — Wallet funcional** | ✅ Finalizada | Flujo principal: cuentas, movimientos, transferencias, balances, persistencia, import/export y pruebas |
| **2 — Adaptación Android** | ✅ Finalizada | Optimización para Android y resolución de problemas específicos de plataforma |
| **3 — Pruebas de uso real** | ✅ Finalizada | Detección y corrección de problemas de navegación, rendimiento y UX |
| **4 — Reorganización visual** | ✅ Finalizada | Rediseño completo de UI/UX, nuevos formularios y enfoque en productividad |
| **5 — Estadísticas y predicciones** | ⏸️ Pospuesta | Llevar gráficos, estadísticas y predicciones a una futura ventana de análisis |
| **6 — Deudas** | ✅ Finalizada | Deudas como cuentas, cargos por categoría, imputación de intereses, conversión de cuenta a deuda y deudas cerradas |
| **7 — Inversiones** | ⏳ Pendiente | Inversiones, objetivos de ahorro y seguimiento de rendimiento |
| **8 — Automatización** | ⏳ Pendiente | Pagos automáticos, generación de reportes periódicos, alertas |
| **9 — Pulido técnico** | ⏳ Pendiente | Optimizaciones, simplificaciones y mejora de estabilidad |
| **10 — Prueba final** | ⏳ Pendiente | Verificación prolongada de estabilidad post-desarrollo |

---

## 💡 Filosofía del proyecto

Mi Wallet se desarrolla de forma **incremental y pragmática**:

- Las decisiones de diseño se revisan cuando la experiencia práctica demuestra mejoras posibles
- El objetivo es **calidad sobre cantidad** de funcionalidades
- Enfoque: herramienta **rápida, práctica y agradable** para uso diario
- Automatización progresiva de tareas repetitivas

> *No se trata de agregar features, sino de construir algo que realmente funcione y sea un placer usar.*

---

## ❓ FAQ

### 📊 Privacidad y seguridad

**P: ¿Dónde se almacenan mis datos?**
R: Todos los datos se guardan **localmente** en tu dispositivo (Android, escritorio, etc.). No se envía información a servidores externos ni se utiliza base de datos remota.

**P: ¿Es seguro usar esta aplicación?**
R: Los datos se almacenan localmente en tu dispositivo con acceso protegido por el sistema operativo. Sin embargo, recomiendo realizar backups periódicos usando la función de exportación.

**P: ¿Puedo hacer backup de mis datos?**
R: Sí, puedes exportar todos tus datos a CSV en cualquier momento desde la sección de Configuración. También puedes importarlos posteriormente.

### 💻 Instalación y uso

**P: ¿Necesito conexión a internet?**
R: No. La aplicación funciona completamente offline. La conexión a internet es opcional.

**P: ¿Puedo usar Mi Wallet en múltiples dispositivos?**
R: Actualmente, los datos se sincronizan manualmente mediante export/import CSV. La sincronización automática podría ser una funcionalidad futura.

**P: ¿Qué requisitos de espacio en disco necesito?**
R: La aplicación ocupa aproximadamente 203 MB. El almacenamiento de datos adicionales es de aproximadamente 200 megas adicionales.

### 🐛 Problemas y solución

**P: ¿Cómo reporto un bug o sugiero una mejora?**
R: Abre un [issue en GitHub](https://github.com/BJhojan/project-wallet-apk/issues/new) con descripción detallada. Consulta también el [roadmap](#-roadmap) para ver si ya está planeado.

**P: ¿Se pierden los datos si desinstalo la app?**
R: En Android, los datos se conservan si mantienes el almacenamiento de datos. En escritorio, verifica la ubicación de los archivos antes de desinstalar.

---

## 📝 Licencia

Este proyecto está licenciado bajo **MIT License with Commons Clause Condition**.

### ¿Qué significa esto?

#### ✅ **Está permitido:**
- Usar el software libremente
- Modificar el código fuente
- Distribuir copias
- Uso personal y educativo, respetando las condiciones de la licencia

#### ❌ **No está permitido:**
- **Vender el software** o un producto cuyo valor derive sustancialmente de él
- Ofrecerlo como servicio comercial basado en su funcionalidad
- Cobrar por servicios de hosting, consultoría o soporte relacionados con el software

Para los términos completos, consulta el archivo [`LICENSE`](LICENSE).

---

## 🤝 Contribuciones y soporte

¿Encontraste un bug o tienes una sugerencia?

- **Reportar un issue:** [Abre un issue aquí](https://github.com/BJhojan/project-wallet-apk/issues/new/choose)
- **Ver issues abiertos:** [Consulta issues activos](https://github.com/BJhojan/project-wallet-apk/issues)
- **Consultar el roadmap:** Revisa la sección [Roadmap](#-roadmap) para ver qué está planeado

**Notas importantes:**
- Este es un proyecto personal en desarrollo continuo
- Las contribuciones son bienvenidas pero primero consulta los issues abiertos
- Respeta la filosofía del proyecto: calidad sobre cantidad


**Última actualización:** Septiembre 2026 | **Mantenedor:** [BJhojan](https://github.com/BJhojan)