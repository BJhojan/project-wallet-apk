# Mi Wallet

Aplicación personal de finanzas hecha con Python y Flet para controlar cuentas, movimientos, transferencias y balances desde una interfaz visual. Funciona con persistencia local, sin depender de un backend externo ni de una base de datos remota.

Su objetivo es concentrar en una sola cartera la información financiera del usuario y ofrecer una consulta rápida del estado actual, el historial y predicciones.

## Vista previa

Las capturas reales de la aplicación se encuentran en la carpeta [`imagenes/`](imagenes/).

| Inicio | Movimientos | Transferencias |
| --- | --- | --- |
| ![Pantalla principal](imagenes/Pantalla%20principal.png) | ![Resumen de movimientos](imagenes/Resumen%20de%20movimientos.png) | ![Transferencias](imagenes/Transferencias.png) |

## Funcionalidades

### Cuentas y balance

- Crear, editar y eliminar cuentas.
- Clasificar cuentas por tipo, como efectivo, banco, deudas, inversiones o ahorros.
- Crear nuevos tipos de cuenta.
- Consultar el saldo individual y el balance global de la wallet.

### Movimientos

Cada ingreso o gasto incluye cuenta, tipo, categoría, descripción, importe y fecha. Los registros se pueden consultar aplicando filtros por cuenta, tipo, categoría y rango de fechas.

La `Wallet` es la responsable de conservar y coordinar el historial de movimientos; las cuentas ya no almacenan el historial por separado.

### Transferencias

Una transferencia relaciona una cuenta de origen con una cuenta de destino y mantiene la trazabilidad mediante un identificador compartido. En la interfaz se muestra como **una sola tarjeta de transferencia**, aunque afecte al saldo de dos cuentas, para evitar presentarla como dos movimientos independientes.

### Importación y exportación

La aplicación permite importar y exportar información en CSV. Actualmente, la importación de archivos de Money Manager requiere este formato fijo de columnas, separadas por `;`:

```text
account;category;currency;amount;ref_currency_amount;type;payment_type;payment_type_local;note;date;gps_latitude;gps_longitude;gps_accuracy_in_meters;warranty_in_month;transfer;payee;labels;envelope_id;custom_category
```

También permite exportar el estado actual de la wallet para crear copias de respaldo o trabajar con los datos fuera de la aplicación.

### Predicciones

La app ofrece una vista previa de predicciones basada en los datos registrados para ayudar a anticipar la evolución financiera.

## Flujo principal

1. Crear o seleccionar las cuentas financieras.
2. Registrar ingresos, gastos o transferencias.
3. Consultar balances, movimientos y filtros.
4. Revisar las predicciones disponibles.
5. Guardar los cambios automáticamente en el almacenamiento local.
6. Importar o exportar datos cuando sea necesario.

## Arquitectura

El proyecto separa la interfaz, la lógica de negocio, el dominio y la persistencia:

```text
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

- **Modelos:** representan `Account`, `Movement` y `Wallet`.
- **Servicios:** coordinan cuentas, movimientos, transferencias, balances y predicciones.
- **Persistencia:** guarda los datos localmente en JSON y gestiona el intercambio mediante CSV.
- **Interfaz:** Flet organiza la navegación, las ventanas, los formularios y las vistas de consulta.
- **Pruebas:** cubren el dominio, transferencias, navegación, importación y componentes de la interfaz.

## Tecnologías

- Python 3.11.3
- Flet 0.86.5
- JSON para persistencia local
- CSV para importación y exportación
- Pytest para pruebas automatizadas

## APK para Android

En la carpeta download.zip encontraras la APK para descargar directamente en android y utilizar.

La versión de escritorio sigue pendiente de un ajuste visual específico; actualmente la interfaz está orientada principalmente a la experiencia móvil Android.

## Estado del proyecto

Mi Wallet ya cuenta con el flujo central de gestión financiera, persistencia local, transferencias, filtros, importación/exportación y pruebas automatizadas. Las siguientes mejoras serán implementar la creación de nuevos tipos de cuenta específicos como deudas o inversiones con cálculos de intereses automáticos y análisis financieros mas detallados.
