# Estado y actualizaciones

Repositorio público. Lo lee cada instalación: no pongas fichas, DNI, claves ni el código de la aplicación.

El programa pide:

- `status.json` — licencias anuladas, y módulos encendidos o apagados
- `updates/latest.json` — versión del instalador. Al abrir, el programa la compara y, si hay una más nueva, pide reiniciar

Los módulos (pacientes, agenda, pruebas, inventario, facturas) van todos en el instalador. La licencia y `status.json` deciden cuáles se ven.

## Qué no va aquí

- Código del programa (repo privado)
- La clave privada Ed25519 (`%USERPROFILE%\.audiem\license-keys\ed25519.pem`)
- El JSON de licencia de un gabinete (`*.license.json`): se carga en Ajustes

## Primera vez

Desde el repo del código, con la clave privada ya creada:

```powershell
node scripts/license-sign.mjs status --empty --out "C:\Users\dasej\Desktop\audiem-licenses\status.json"
```

## Encender o apagar un módulo

No hace falta reinstalar. Firma el estado y sube `status.json` a `main`. El programa lo mira al entrar.

```powershell
node scripts/license-sign.mjs status --id <uuid> --on tests --merge "C:\Users\dasej\Desktop\audiem-licenses\status.json" --out "C:\Users\dasej\Desktop\audiem-licenses\status.json"
node scripts/license-sign.mjs status --id <uuid> --off billing --merge "C:\Users\dasej\Desktop\audiem-licenses\status.json" --out "C:\Users\dasej\Desktop\audiem-licenses\status.json"
```

`--on tests` muestra Pruebas. `--off billing` esconde Facturas. `--revoke` deja la licencia en solo consulta.

## Publicar una versión

Cuando el instalador de Windows esté firmado por Tauri:

1. Copia `updates/latest.example.json` a `updates/latest.json`.
2. Pon la versión nueva, la firma y la URL del `.nsis.zip`. El paquete se llama `Gabinete_<version>_x64-setup.nsis.zip`.
3. Sube ese archivo a `main` y el zip a un release.

Hasta que `updates/latest.json` exista, el programa abre igual y no pide actualizar.
