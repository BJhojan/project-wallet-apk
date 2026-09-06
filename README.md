# Mi Wallet

[![Python 3.11+](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://www.python.org/)
[![Flet 0.86.5](https://img.shields.io/badge/Flet-0.86.5-green.svg)](https://flet.dev/)
[![License: MIT with Commons Clause](https://img.shields.io/badge/License-MIT%20with%20Commons%20Clause-orange.svg)](LICENSE)
[![Private Repository](https://img.shields.io/badge/Status-Private-red.svg)]()

Aplicación personal de finanzas desarrollada con **Python y Flet** para gestionar cuentas, movimientos, transferencias y balances desde una interfaz visual intuitiva.

La aplicación funciona con **persistencia local**, sin depender de un backend externo ni de una base de datos remota.

**Objetivo:** Concentrar en una única cartera la información financiera del usuario y ofrecer una consulta rápida del estado actual, historial e indicadores para facilitar la gestión diaria.

---

## 📋 Tabla de contenidos

- [Vista previa](#-vista-previa)
- [Funcionalidades](#-funcionalidades)
- [Descarga e instalación](#-descarga-e-instalación)
- [Uso](#-flujo-de-uso)
- [Contenido del repositorio](#-contenido-del-repositorio)
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

### 📈 Predicciones y análisis

La aplicación incorpora predicciones basadas en datos registrados para una referencia sobre la evolución financiera esperada.

> ⚠️ **Estado:** Actualmente en desarrollo. Será ampliada con herramientas de análisis más detalladas.

### ⚙️ Configuración

Personaliza distintos aspectos de la wallet:
- Privacidad (ocultar/mostrar saldos)
- Preferencias de gráficos
- Categorías y tipos de cuenta
- Opciones generales de la aplicación

### 📥 Importación y exportación

**Importación:** Soporta archivos CSV en formato **Money Manager**

El archivo debe contener las siguientes columnas separadas por comas:

```
account	category	currency	amount	ref_currency_amount	type	payment_type	payment_type_local	note	date	gps_latitude	gps_longitude	gps_accuracy_in_meters	warranty_in_month	transfer	payee	labels	envelope_id	custom_category
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
3. Consultar balances y registros mediante filtros
4. Revisar información en la pantalla de inicio
5. Revisar predicciones disponibles
6. Personalizar desde Configuración
7. Los cambios se guardan automáticamente en almacenamiento local
8. Importar o exportar información cuando sea necesario

---

## 📦 Descarga e instalación

Este repositorio distribuye la aplicación mediante [`download.zip`](download.zip). El ZIP contiene el archivo `project.apk`.

1. Descarga [`download.zip`](download.zip) desde este repositorio.
2. Extrae el archivo `project.apk` en tu dispositivo Android.
3. Transfiere el APK al dispositivo Android si lo descargaste en otro equipo.
4. Abre el APK y autoriza la instalación desde esa fuente cuando Android lo solicite.

> Android puede mostrar una advertencia al instalar un APK descargado fuera de Google Play. Instálalo únicamente si confías en el origen del archivo.

### Contenido del repositorio

```text
.
├── download.zip       # Paquete de distribución que contiene project.apk
├── imagenes/          # Capturas de la aplicación
├── LICENSE            # Licencia MIT modificada con Commons Clause
└── README.md          # Documentación del proyecto
```

El código fuente no está incluido en este paquete de distribución. Por tanto, las instrucciones de compilación con Python/Flet no aplican a esta copia del repositorio.

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

**Características implementadas:**
- ✅ Gestión de cuentas (crear, editar, eliminar)
- ✅ Registros de movimientos (ingresos y gastos)
- ✅ Transferencias entre cuentas
- ✅ Balances y consultas
- ✅ Filtros por período, cuenta, tipo y categoría
- ✅ Predicciones iniciales
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
| **5 — Estadísticas y predicciones** | 🟢 En desarrollo | Ampliación de gráficos, estadísticas y análisis con Flet Charts |
| **6 — Deudas e inversiones** | ⏳ Pendiente | Sistemas específicos para deudas, inversiones, intereses y objetivos de ahorro |
| **7 — Automatización** | ⏳ Pendiente | Pagos automáticos, generación de reportes periódicos, alertas |
| **8 — Pulido técnico** | ⏳ Pendiente | Optimizaciones, simplificaciones y mejora de estabilidad |
| **9 — Prueba final** | ⏳ Pendiente | Verificación prolongada de estabilidad post-desarrollo |

---

## 💡 Filosofía del proyecto

Mi Wallet se desarrolla de forma **incremental y pragmática**:

- Las decisiones de diseño se revisan cuando la experiencia práctica demuestra mejoras posibles
- El objetivo es **calidad sobre cantidad** de funcionalidades
- Enfoque: Herramienta **rápida, práctica y agradable** para uso diario
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
R: La aplicación ocupa aproximadamente 150-200 MB. El almacenamiento de datos adicionales depende del número de movimientos registrados (Actualmente menos de 1 mega por 3800 registros).

### 🐛 Problemas y solución

**P: ¿Cómo reporto un bug o sugiero una mejora?**
R: Abre un [issue en GitHub](https://github.com/BJhojan/project-wallet/issues/new) con descripción detallada. Consulta también el [roadmap](#-roadmap) para ver si ya está planeado.

**P: ¿Se pierden los datos si desinstalo la app?**
R: En Android, los datos se conservan si mantienes el almacenamiento de datos. En escritorio, verifica la ubicación de los archivos antes de desinstalar.

---

## 📝 Licencia

El material original de este proyecto se distribuye bajo la **Licencia MIT
con la condición Commons Clause v1.0**, incluida en [`LICENSE`](LICENSE).

La condición Commons Clause modifica el permiso MIT de venta: no se concede el
derecho a vender el software ni un producto o servicio cuyo valor derive total
o sustancialmente de su funcionalidad. Por tanto, esta no es la Licencia MIT
estándar ni una licencia aprobada por la OSI.

### ¿Qué significa esto?

#### ✅ **Está permitido:**
- Usar el software libremente
- Modificar el código fuente
- Distribuir copias
- Uso personal y educativo

#### ❌ **No está permitido:**
- **Vender el software**
- Vender un producto o servicio cuyo valor derive total o sustancialmente de
	la funcionalidad del software, incluido ofrecerlo como servicio o cobrar por
	hosting basado en este código

Las dependencias de terceros, como Python y Flet, conservan sus propias
licencias. Esta licencia solo cubre el material original de este proyecto.

Para los términos completos, consulta el archivo [`LICENSE`](LICENSE).

---

## 🤝 Contribuciones y soporte

¿Encontraste un bug o tienes una sugerencia?

- **Reportar un issue:** [Abre un issue aquí](https://github.com/BJhojan/project-wallet/issues/new/choose)
- **Ver issues abiertos:** [Consulta issues activos](https://github.com/BJhojan/project-wallet/issues)
- **Consultar el roadmap:** Revisa la sección [Roadmap](#-roadmap) para ver qué está planeado

**Notas importantes:**
- Este es un proyecto personal en desarrollo continuo
- Las contribuciones son bienvenidas pero primero consulta los issues abiertos
- Respeta la filosofía del proyecto: calidad sobre cantidad

---

## 📝 Mejoras implementadas en este README

- ✅ Agregados badges de versión, tecnología y licencia
- ✅ Añadida tabla de contenidos para mejor navegación
- ✅ Sección de instalación y configuración detallada
- ✅ Instrucciones para compilar APK de Android
- ✅ Arquitectura mejorada con descripción de directorios
- ✅ FAQ completo sobre privacidad, uso y solución de problemas
- ✅ Enlaces directos para reportar issues
- ✅ Actualizado roadmap con fase de automatización
- ✅ Tabla de tecnologías mejorada con propósitos
- ✅ Clarificación sobre compatibilidad de plataformas
- ✅ Sección de Licencia con explicación de MIT modificada con Commons Clause

---

**Última actualización:** Septiembre 2026 | **Mantenedor:** [BJhojan](https://github.com/BJhojan)
