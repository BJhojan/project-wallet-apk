# Mi Wallet

Aplicación personal de finanzas desarrollada con **Python y Flet** para gestionar cuentas, movimientos, transferencias y balances desde una interfaz visual intuitiva.

La aplicación funciona con **persistencia local**, sin depender de un backend externo ni de una base de datos remota.

**Objetivo:** Concentrar en una única cartera la información financiera del usuario y ofrecer una consulta rápida del estado actual, historial e indicadores para facilitar la gestión diaria.

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

El archivo debe contener las siguientes columnas separadas por tabulación (tab):

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

## 🏗️ Arquitectura

El proyecto separa la interfaz, lógica de negocio, dominio y persistencia:

```
project/
├── main.py
├── backend/
│   ├── models/
│   ├── persistence.py
│   ├── import_export.py
│   └── wallet_services.py
├── services/
├── persistence/
├── ui/
├── tests/
├── data/
└── build/
```

**Componentes principales:**
- **Modelos:** `Account`, `Movement` y `Wallet`
- **Servicios:** Coordinan cuentas, movimientos, transferencias, balances y predicciones
- **Persistencia:** Administra almacenamiento local e intercambio CSV
- **Interfaz:** Flet organiza navegación, formularios, componentes visuales y vistas
- **Pruebas:** Cobertura de dominio, transferencias, navegación, importación y componentes UI

> La interfaz fue reconstruida para priorizar experiencia de uso y reducir dependencia del historial permanente de movimientos.

---

## 🛠️ Tecnologías

| Tecnología | Versión |
|---|---|
| Python | 3.11.3 |
| Flet | 0.86.5 |
| Persistencia | JSON |
| Importación/Exportación | CSV |
| Testing | Pytest |

---

## 📲 Descargar

La aplicación puede descargarse desde [`download.zip`](download.zip) en este repositorio.

**Plataformas:**
- ✅ **Android:** Versión optimizada y completa
- 🟡 **Escritorio:** Funcional pero con ajustes visuales pendientes

---

## 🔧 Compatibilidad

- **Diseño principal:** Android
- **Base de código:** Multiplataforma mediante Flet
- **Pruebas específicas:** Resueltas diferencias en interacción, selección de archivos y persistencia entre plataformas

---

## 📋 Estado del proyecto

**Características implementadas:**
- ✅ Gestión de cuentas
- ✅ Registros de movimientos
- ✅ Transferencias entre cuentas
- ✅ Balances y consultas
- ✅ Filtros por período
- ✅ Predicciones iniciales
- ✅ Configuración personalizable
- ✅ Persistencia local
- ✅ Importación/Exportación CSV
- ✅ Suite de pruebas automatizadas

**Evolución continua:** El proyecto evoluciona a partir de pruebas de uso real, enfocándose en ampliar análisis financiero, funcionalidades para deudas e inversiones, y cálculos automáticos.

---

## 🚀 Roadmap

| Fase | Estado | Descripción |
|---|---|---|
| **1 — Wallet funcional** | ✅ Finalizada | Flujo principal: cuentas, movimientos, transferencias, balances, persistencia, import/export y pruebas |
| **2 — Adaptación Android** | ✅ Finalizada | Optimización para Android y resolución de problemas específicos de plataforma |
| **3 — Pruebas de uso real** | ✅ Finalizada | Detección y corrección de problemas de navegación, rendimiento y UX |
| **4 — Reorganización visual** | ✅ Finalizada | Rediseño completo de UI/UX, nuevos formularios y enfoque en productividad |
| **5 — Estadísticas y predicciones** | 🟢 En desarrollo | Ampliación de gráficos, estadísticas y análisis |
| **6 — Deudas e inversiones** | ⏳ Pendiente | Sistemas específicos para deudas, inversiones y objetivos de ahorro |
| **7 — Pulido técnico** | ⏳ Pendiente | Optimizaciones, simplificaciones y mejora de estabilidad |
| **8 — Prueba final** | ⏳ Pendiente | Verificación prolongada de estabilidad post-desarrollo |

---

## 💡 Filosofía del proyecto

Mi Wallet se desarrolla de forma **incremental y pragmática**:

- Las decisiones de diseño se revisan cuando la experiencia práctica demuestra mejoras posibles
- El objetivo es **calidad sobre cantidad** de funcionalidades
- Enfoque: Herramienta **rápida, práctica y agradable** para uso diario

> *No se trata de agregar features, sino de construir algo que realmente funcione y sea un placer usar.*

---

## 📝 Notas y mejoras sugeridas

- Considera agregar badges de versión y licencia en la cabecera
- Documentación de instalación para desarrollo podría ser útil
- Links a issues/contributing podrían mejorar la colaboración
- Considera agregar una sección de FAQ sobre datos y privacidad
