# Audiem — estado y actualizaciones

Repositorio **público**. Lo lee cada instalación: no pongas fichas, DNI, claves ni el código de la aplicación.

El programa pide:

- `status.json` — licencias anuladas o con un módulo apagado
- `updates/latest.json` — versión del instalador (actualizador de Tauri)

Los módulos (pacientes, agenda, pruebas, inventario, facturas) no son otro repositorio: van en el archivo de licencia de cada gabinete.

## Qué no va aquí

- Código de Audiem (eso es el repo privado `audiem`)
- La clave privada Ed25519 (`%USERPROFILE%\.audiem\license-keys\ed25519.pem`)
- El JSON de licencia de un gabinete (`*.license.json`): se entrega a mano y se carga en Ajustes

## Primera vez

Desde el repo del código, con la clave privada ya creada:

```powershell
node scripts/license-sign.mjs status --empty --out "C:\Users\dasej\Desktop\audiem-licenses\status.json"
```

Copia `updates/latest.example.json` a `updates/latest.json` cuando tengas un instalador firmado y cambia la URL y la firma. Hasta entonces el actualizador no encontrará paquete.

## Anular o apagar un módulo

```powershell
node scripts/license-sign.mjs status --id <uuid-de-la-licencia> --revoke --merge "C:\Users\dasej\Desktop\audiem-licenses\status.json" --out "C:\Users\dasej\Desktop\audiem-licenses\status.json"
```

`--off billing` apaga facturas sin anular el resto. Sube el `status.json` nuevo a `main`.
