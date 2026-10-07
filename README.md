# Voranix Modpacks

Catálogo de modpacks que muestra **Voranix Launcher** en su sección *Modpacks*
(el ícono del bloque en la barra lateral).

Este repositorio **solo guarda metadatos**. El launcher descarga cada modpack y sus
mods directamente desde CurseForge, así no se re-publican archivos de otros autores.

## Cómo agregar un modpack

1. Busca el modpack en CurseForge y abre la pestaña **Files**.
2. Elige la versión (debe ser el zip del modpack, con `manifest.json` adentro).
3. Copia el número del final de la URL del archivo: ese es el `fileId`.
   El `projectId` aparece en la columna derecha de la página del proyecto.
4. Agrega una entrada en `modpacks.json` y súbela a `main`.

El launcher lee `modpacks.json` al abrir *Modpacks* (GitHub puede tardar unos
minutos en mostrar los cambios).

```json
{
  "id": "rlcraft",
  "name": "RLCraft",
  "description": "Texto corto que verán los jugadores.",
  "author": "Shivaxi",
  "version": "2.9.3",
  "minecraft": "1.12.2",
  "loader": "forge-14.23.5.2860",
  "recommendedRamGB": 8,
  "logo": "logos/rlcraft.png",
  "banner": "banners/rlcraft.webp",
  "website": "https://www.curseforge.com/minecraft/modpacks/rlcraft",
  "source": {
    "type": "curseforge",
    "projectId": 285109,
    "fileId": 4612979,
    "fileName": "RLCraft 1.12.2 - Release v2.9.3.zip",
    "size": 51324367
  }
}
```

| Campo | Obligatorio | Notas |
|---|---|---|
| `id` | sí | Minúsculas, números, `-` o `_`. Es el nombre de la carpeta del modpack; no lo cambies después de publicarlo. |
| `name`, `version` | sí | Al cambiar `version` (y `fileId`) los jugadores ven **Actualizar**. |
| `minecraft` | sí | Versión de Minecraft del pack, ej. `1.12.2`. |
| `loader` | sí | Por ahora solo Forge: `forge-<versión>` (está en el `manifest.json` del pack). |
| `source.projectId`, `source.fileId` | sí (CurseForge) | IDs de CurseForge. Para packs propios ver abajo (`"type": "url"`). |
| `source.size` | no | Tamaño exacto del zip en bytes; si está, el launcher comprueba que la descarga llegó completa. |
| `logo` | no | Ícono cuadrado (ideal 256–512 px, PNG con fondo transparente). URL `https://...` o ruta dentro de este repo (ej. `logos/rlcraft.png`). Sin logo se muestra un bloque. |
| `banner` | no | Fondo de la tarjeta, horizontal (ej. 1920×720). Se oscurece a la izquierda para que se lea el texto. Mismo formato que `logo` (ej. `banners/rlcraft.webp`). |
| `description`, `author`, `recommendedRamGB`, `website` | no | Se muestran en la tarjeta del modpack. |

## Packs propios (no publicados en CurseForge)

Para un pack nuestro, el zip va en `packs/` y la entrada usa `"type": "url"`:

```json
"source": {
  "type": "url",
  "url": "packs/voranix-modpack-test.zip",
  "size": 18398806
}
```

El zip tiene el formato de una exportación de CurseForge (`manifest.json` + `overrides/`).
Los mods de terceros **no van dentro del zip**:

- Los de CurseForge, en `manifest.json` → `files` (como cualquier exportación).
- Los que solo están en Modrinth, en `manifest.json` → `externalFiles`, cada uno con
  `url` (solo `https://cdn.modrinth.com/...`), `fileName` y `sha1`. El launcher
  comprueba el `sha1` al descargarlos.
- En `overrides/mods` solo van mods propios (ej. `voranix-1.9.1.jar`).

No subas carpetas personales del `.minecraft`: cachés de Essential, mapas de
JourneyMap (`journeymap/data`) ni archivos `.log`.

## Dónde se instalan

Cada modpack tiene su propia carpeta (mundos, configs y mods separados):
`%APPDATA%\voranix-launcher\modpacks\<id>`.
