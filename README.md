# NodePilot — Microsoft Store (distribución EXE/MSI)

> ⚠️ **Este repositorio es EXCLUSIVAMENTE para publicar NodePilot en la Microsoft Store.**
> Su único fin es **alojar el instalador firmado** (`NodePilot-Setup.exe`) que la Store descarga
> desde la *Dirección URL del paquete* configurada en Partner Center (envío de tipo **EXE/MSI**).
> **No contiene el código fuente** de NodePilot.

## Por qué existe

En los envíos tipo **EXE/MSI**, la Microsoft Store **no aloja el instalador**: lo sirve desde una
URL que controlas tú. Aquí usamos **GitHub Releases** como host **versionado e inmutable**, que es
justo lo que Microsoft exige (una URL segura, versionada, cuyo binario no cambia tras el envío).

## Cómo funciona el versionado

- Cada versión de la app = una **Release** con su tag: `v1.0.0`, `v1.0.1`, …
- El asset `NodePilot-Setup.exe` de esa release da la **URL de paquete** para Partner Center:

  ```
  https://github.com/Elgeryy1/nodepilot-msstore/releases/download/v1.0.0/NodePilot-Setup.exe
  ```

- Al sacar una versión nueva: **nuevo tag → nueva release → nueva URL**, y en Partner Center usas
  *"Actualizar envío"* para pegar la nueva URL.
- **Nunca** reemplaces un asset ya publicado: la Store exige que el binario de una URL enviada no
  cambie. Versión nueva = tag nuevo = URL nueva.

## Qué poner en Partner Center (Detalles del paquete)

| Campo | Valor |
|---|---|
| **Dirección URL del paquete** | URL del asset de la release (ver arriba) |
| **Arquitectura** | `x64` |
| **Parámetros del instalador** | `/VERYSILENT /SUPPRESSMSGBOXES /NORESTART` |
| *"modo silencioso sin modificadores"* | **desmarcado** (el instalador Inno necesita los switches) |

## Requisito imprescindible: firma de código

El `.exe` **debe estar firmado con Authenticode** con un certificado de una CA del *Microsoft
Trusted Root Program* (p.ej. **Azure Trusted Signing**, o Sectigo/DigiCert). La Store **no
re-firma** los EXE/MSI; un certificado **auto-firmado NO pasa** la validación de paquetes.

## Publicar una versión (cuando el `.exe` esté firmado)

```bash
gh release create v1.0.0 NodePilot-Setup.exe \
  --title "NodePilot 1.0.0" \
  --notes "Versión completa de NodePilot para Microsoft Store."
```

Después, copia la URL del asset (`.../releases/download/v1.0.0/NodePilot-Setup.exe`) y pégala en la
*Dirección URL del paquete* de Partner Center.

---

*La edición reducida (MSIX) y el código fuente viven fuera de este repositorio; aquí solo se
distribuye el instalador completo firmado para la Microsoft Store.*
