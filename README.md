# Mi Wallet

Aplicación personal de gestión financiera construida con **Python + Flet**. El proyecto permite administrar cuentas, movimientos y transferencias, con persistencia local, importación/exportación CSV y adaptación para escritorio y Android.

Actualmente la primera versión funcional está terminada y el proyecto se encuentra en la etapa de **pruebas y adaptación de experiencia de usuario**.

---

## Características

### Cuentas

* Crear cuentas.
* Modificar nombre, tipo, descripción y balance.
* Eliminar cuentas.
* Unificar dos cuentas.
* Mantener balances individuales y balance global.
* Crear movimientos de ajuste automáticamente cuando corresponde.
* Autocompletar descripciones de cuentas.

### Movimientos

* Crear movimientos de ingreso y gasto.
* Modificar movimientos.
* Eliminar movimientos.
* Categorías, etiquetas y descripciones.
* Autocompletado.
* Filtros combinados por:

  * cuenta
  * tipo
  * categoría
  * fecha
* Orden cronológico.
* Carga por lotes.

### Transferencias

Las transferencias se representan internamente como dos movimientos relacionados mediante un `transfer_id`.

Permiten:

* Crear transferencias.
* Modificar transferencias.
* Eliminar transferencias.
* Filtrar por:

  * cuenta origen
  * cuenta destino
  * fecha
* Mostrar una única tarjeta visual por transferencia.

### Unificación de cuentas

Permite absorber una cuenta dentro de otra.

La cuenta que se conserva mantiene su:

* nombre
* tipo
* descripción
* identidad

Los movimientos de la cuenta absorbida pasan a la cuenta conservada y las transferencias entre ambas cuentas se eliminan.

Las transferencias con cuentas externas mantienen su `transfer_id`.

La operación requiere confirmación y es irreversible.

### Persistencia

La aplicación utiliza un archivo JSON local para conservar los datos.

El flujo de carga está dividido:

1. Se carga inicialmente la información resumida de la wallet.
2. Los movimientos se cargan posteriormente de forma asíncrona.
3. Los movimientos visuales se generan por lotes.
4. Los `Card` se mantienen almacenados y se reutilizan entre ventanas.

La aplicación también guarda los cambios localmente.

### Importación y exportación

Actualmente existe soporte para CSV.

**Importación:**

* Formato CSV definido por la aplicación:
* Solo admite el siguiente formato:
[account,	category,	currency,	amount, ref_currency_amount,	type,	payment_type,	payment_type_local,	note,	date,	gps_latitude,	gps_longitude,	gps_accuracy_in_meters,	warranty_in_month,	transfer,	payee,	labels,	envelope_id,	custom_category]

* Conversión de filas de transferencia a la estructura interna de la wallet.
* Procesamiento con reporte de progreso.
* Soporte para grandes cantidades de registros, aunque el rendimiento de importaciones extremadamente grandes todavía está pendiente de optimización.

**Exportación:**

* CSV.
* Escritorio: escritura mediante ruta.
* Android: generación del CSV en memoria y entrega mediante `FilePicker` utilizando `src_bytes`.

### Android

La aplicación puede ejecutarse directamente en Android mediante APK.

Durante las pruebas se verificaron:

* carga de datos
* drawer
* navegación
* movimientos
* transferencias
* importación
* exportación

También se realizaron adaptaciones específicas para móvil.

---

# Arquitectura

El proyecto está dividido principalmente en cuatro capas:

```text
models/
services/
persistence/
ui/
```

La idea general es:

```text
UI
 │
 ▼
WindowManager
 │
 ▼
WalletService / WalletManager
 │
 ├── AccountService
 ├── MovementService
 ├── TransferService
 └── PersistenceService
 │
 ▼
Models
```

---

## Models

Contienen el estado y la lógica principal del dominio.

```text
models/
├── account.py
├── movement.py
└── wallet.py
```

### `Wallet`

Administra:

* cuentas
* movimientos
* transferencias
* balances
* IDs
* filtros
* autocomplete

También contiene la representación general de los elementos financieros.

### `Account`

Representa una cuenta y sus movimientos.

### `Movement`

Representa un ingreso, gasto o parte de una transferencia.

---

# Services

```text
services/
├── account_service.py
├── movement_service.py
├── persistence_service.py
├── preset_services.py
├── transfer_service.py
└── wallet_service.py 
```

## `WalletService`

Funciona como fachada entre la UI y los servicios del dominio.

Por ejemplo:

```python
manager.wallet.account_create(...)
manager.wallet.movement_create(...)
manager.wallet.transfer_create(...)
```

## `AccountService`

Gestiona:

* creación
* consulta
* modificación
* eliminación
* unificación de cuentas

## `MovementService`

Gestiona:

* creación
* modificación
* eliminación
* filtrado
* autocomplete

## `TransferService`

Gestiona:

* creación
* modificación
* eliminación
* consulta
* filtrado

## `PersistenceService`

Gestiona:

* persistencia local
* importación CSV
* exportación CSV
* carga inicial
* carga posterior de movimientos

---

# Persistence

```text
persistence/
├── csv_manager.py
├── excel_manager.py
└── json_manager.py
```

`JsonManager` se utiliza para la persistencia local.

`CsvManager` gestiona la lectura y escritura CSV.

Existe además `ExcelManager` como soporte de persistencia disponible en el proyecto, aunque el flujo principal actual utiliza CSV.

---

# UI

```text
ui/
├── forms/
├── presets/
├── view/
└── window_manager.py
```

## WindowManager

Es responsable de:

* mantener la instancia de `WalletService`
* registrar las ventanas disponibles
* administrar el drawer
* administrar `FilePicker`
* cambiar entre ventanas
* gestionar el contenedor principal
* mostrar el indicador de carga

Actualmente las ventanas registradas incluyen:

```text
## View windows
"Home": Home,
"AccountView": AccountView,
"MovementView": MovementView,
"TransferView": TransferView,
"ImportView": ImportView,
"ExportView": ExportView,
## Create windows
"AccountCreate": AccountCreate,
"MovementCreate": MovementCreate,
"TransferCreate": TransferCreate,
## Modify windows
"MovementModify": MovementModify,
"TransferModify": TransferModify,
"AccountModify": AccountModify,
"AccountMerge": AccountMerge
```

---

## Views

```text
ui/view/
├── home_view.py
├── account_view.py
├── movements_view.py
├── transfer_view.py
├── import_view.py
└── export_view.py
```

Las vistas manejan principalmente:

* composición de la interfaz
* navegación
* filtros
* interacción del usuario

---

# MovementManager

Una de las piezas importantes de la interfaz es:

```text
ui/presets/movement_preset.py
```

`MovementManager` mantiene una única instancia del `ListView` de movimientos:

```python
self.movements
```

y caches de tarjetas:

```python
self.movement_cards
self.transfer_cards
```

La idea es:

```text
Aplicación
   │
   ▼
MovementManager
   │
   ├── ListView
   ├── Movement Cards
   └── Transfer Cards
```

Cuando se crea un movimiento nuevo:

```text
backend
 ↓
insert_movement()
 ↓
crear solamente el nuevo Card
```

Cuando se modifica:

```text
backend
 ↓
update_movement()
 ↓
actualizar/reubicar ese Card
```

Cuando se elimina:

```text
backend
 ↓
delete_movement()
 ↓
eliminar solamente ese Card
```

El mismo patrón se aplica a transferencias.

---

# Elementos visuales

La UI está siendo reorganizada para centralizar componentes visuales.

Actualmente:

```text
ui/presets/
├── elements.py
├── controls/
└── window_presets/
```

`elements.py` contiene componentes reutilizables como:

```python
balance_text()
account_card()
movement_card()
transfer_card()
```

La intención es evitar crear el mismo elemento visual disperso por diferentes ventanas y facilitar cambios de estilo futuros.

La organización también incluye controles separados para:

```text
forms
navigation
views
cards
home
```

---

# Formularios

```text
ui/forms/
├── account_forms/
├── movement_forms/
├── transfer_forms/
└── movement_form_preset.py
```

Los formularios están divididos entre creación y modificación.

Ejemplo:

```text
account_forms/
├── account_create_form.py
├── account_modify_form.py
└── account_merge_form.py

movement_forms/
├── movement_create_form.py
└── movement_modify_form.py

transfer_forms/
├── transfer_create_form.py
└── transfer_modify_form.py
```

---

# Navegación

La aplicación utiliza un `NavigationDrawer` compartido.

Actualmente contiene:

```text
Inicio
Movimientos
Transferencias
Importar
Exportar
```

Las ventanas mantienen parámetros como:

```python
return_window
return_account_name
```

para regresar al contexto desde el que fueron abiertas.

---

# Pruebas

El proyecto utiliza **pytest**.

```text
tests/
├── test_account.py
├── test_movement.py
├── test_transfer.py
└── test_wallet.py
```

Actualmente existen aproximadamente **58 pruebas**.

Las pruebas cubren principalmente:

### Cuentas

* creación
* duplicados
* balances
* modificación
* renombrado
* eliminación
* filtrado

### Movimientos

* creación
* valores
* balances
* IDs
* aislamiento por cuenta
* modificación
* eliminación
* filtros
* autocomplete

### Transferencias

* creación
* validación de cuentas
* valores
* modificación
* eliminación
* recuperación
* balances
* conservación del balance global

### Wallet

* balance global
* consultas generales
* cuentas y movimientos

# Ejecución

El punto de entrada es:

```text
main.py
```

La aplicación se inicia con:

```python
import flet as ft

from ui.window_manager import WindowManager


def main(page: ft.Page):

    manager = WindowManager(page)

    manager.change("Home")


if __name__ == "__main__":
    ft.run(main)
```

Por lo tanto, normalmente se ejecuta con:

```bash
python main.py
```

El proyecto utiliza Flet y actualmente ha sido probado con:

```text
flet==0.86.5
```

---

# Compatibilidad

La aplicación está orientada principalmente a:

* Windows/Desktop - Test
* Android

Una de las características principales es que el uso de flet permite mantener la misma base de código para ambas plataformas.

Durante las pruebas se identificaron diferencias específicas de Android.

Por ejemplo, incompatibilidades con `ContextMenuTrigger.DOWN` que no presentó el mismo comportamiento que en escritorio al intentar activar un `ContextMenu` con primaryItems, mientras que `LONG_PRESS` sí funcionó. Debido a que `LONG_PRESS` no resulta ideal para esta aplicación, se optó por no usar `ContextMenu` y en su lugar utilizar `PopupMenuButton` para los menús de creación.

---

# Rendimiento

El principal problema de rendimiento identificado no está en la creación de objetos Python, sino en la cantidad de controles que Flet debe procesar y actualizar.

La carga de movimientos se optimizó utilizando:

* `ListView`
* carga por lotes
* `asyncio`
* reutilización de `Card`
* cache de elementos
* actualización individual de controles

La aplicación ha sido probada con aproximadamente **7.000 transferencias** en la carga inicial sin problemas importantes.

La importación de cantidades muy grandes de movimientos todavía tiene margen de mejora.

La optimización masiva está reservada para una fase posterior del proyecto.

---

# Diseño del proyecto

El proyecto sigue una filosofía de desarrollo incremental.

No se busca optimizar absolutamente todo desde el principio.

La regla actual es:

> **Si funciona y no está causando un problema, no se toca todavía.**

Si una fase revela una limitación técnica concreta, esa parte puede optimizarse localmente.

La refactorización completa queda reservada para la fase final, cuando la arquitectura funcional esté establecida y exista suficiente experiencia de uso real.

---

# Roadmap

## Fase 1 — Wallet funcional desde PC

**Estado: ✅ Finalizada**

Objetivos:

* backend
* interfaz
* cuentas
* movimientos
* transferencias
* persistencia
* importación
* exportación
* filtros
* unificación de cuentas
* tests

---

## Fase 2 — Diseño consistente + adaptación Android

**Estado: ✅ Finalizada**

Objetivos:

* establecer una estética consistente
* reorganizar la UI
* centralizar elementos visuales
* mejorar la organización de carpetas
* adaptar la aplicación a Android
* resolver diferencias de interacción
* adaptar escalado y controles
* mejorar compatibilidad móvil

---

## Fase 3 — Test inicial de uso real

**Estado: ⏳ Pendiente**

La aplicación será utilizada durante varios días para detectar problemas que no aparecen durante el desarrollo.

Se evaluarán:

* comodidad
* navegación
* persistencia
* consistencia de datos
* errores
* comportamiento en Android
* funcionamiento de movimientos y transferencias
* problemas de UX

---

## Fase 4 — Reorganización visual y calidad de vida

**Estado: ⏳ Pendiente**

Objetivo: hacer que la aplicación sea más agradable de utilizar y establecer una estética definitiva.

Incluye:

* paleta visual
* jerarquía
* espaciado
* componentes
* mensajes
* estados vacíos
* interacción
* pequeños atajos
* mejoras de usabilidad

---

## Fase 5 — Estadísticas y predicciones

**Estado: ⏳ Pendiente**

Se incorporarán herramientas para analizar la información financiera acumulada y generar estadísticas y predicciones.

El diseño concreto todavía no está definido.

---

## Fase 6 — Deudas y ahorros

**Estado: ⏳ Pendiente**

Se incorporarán sistemas relacionados con:

* deudas
* ahorros
* intereses automáticos
* Pagos manuales y automáticos
* Representación visual de los intereses pagados
* Predicciones adicionales de objetivos

---

## Fase 7 — Optimización masiva

**Estado: ⏳ Fase final**

Será la primera etapa destinada específicamente a revisar profundamente el código.

Objetivos:

* refactorizar
* eliminar duplicaciones
* simplificar la arquitectura
* optimizar rendimiento
* mejorar mantenibilidad
* mantener y ampliar los tests

Esta fase será realizada con la estructura funcional final ya definida.

---

## Fase 8 — Test final

**Estado: ⏳ Final**

Prueba completa de aproximadamente un mes.

La finalidad será verificar que todas las funcionalidades permanezcan estables después de la optimización y de la incorporación de las funcionalidades finales.

---

# Filosofía de desarrollo

El proyecto se desarrolla de forma incremental.

La primera versión se utilizó deliberadamente para descubrir:

* qué funcionalidades eran necesarias
* cómo interactúan entre sí
* qué problemas aparecen con muchos datos
* qué problemas aparecen en Android
* qué partes del diseño resultan incómodas
* qué responsabilidades necesitan refactorización

La intención es lograr un avance constante:

```text
funcionalidad
 ↓
uso real
 ↓
problema
 ↓
corrección específica
 ↓
siguiente fase
 ↓
arquitectura final
 ↓
refactorización masiva
```

---

# Estado actual

La aplicación ya puede utilizarse como una wallet funcional.

Actualmente el proyecto se encuentra entre la **finalización de la adaptación Android** y el inicio de las **pruebas de uso real**.

La prioridad inmediata es comprobar el comportamiento real de la aplicación antes de avanzar hacia estadísticas, predicciones, deudas y ahorros.

---

## Estructura resumida

```text
project/
│
├── main.py
│
├── models/
│   ├── account.py
│   ├── movement.py
│   └── wallet.py
│
├── services/
│   ├── account_service.py
│   ├── movement_service.py
│   ├── persistence_service.py
│   ├── preset_services.py
│   ├── transfer_service.py
│   └── wallet_service.py
│
├── persistence/
│   ├── csv_manager.py
│   ├── excel_manager.py
│   └── json_manager.py
│
├── ui/
│   ├── forms/
│   │   ├── account_forms/
│   │   ├── movement_forms/
│   │   ├── transfer_forms/
│   │   └── movement_form_preset.py
│   │
│   ├── presets/
│   │   ├── controls/
│   │   ├── window_presets/
│   │   ├── elements.py
│   │   └── movement_preset.py
│   │
│   ├── view/
│   │   ├── home_view.py
│   │   ├── account_view.py
│   │   ├── movements_view.py
│   │   ├── transfer_view.py
│   │   ├── import_view.py
│   │   └── export_view.py
│   │
│   └── window_manager.py
│
└── tests/
    ├── test_account.py
    ├── test_movement.py
    ├── test_transfer.py
    └── test_wallet.py
```
