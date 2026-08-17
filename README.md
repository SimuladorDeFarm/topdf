# pdfconversor

Script de terminal para convertir documentos a PDF usando LibreOffice headless. Soporta conversión de archivos individuales o carpetas completas.

**Formatos soportados:** `doc`, `docx`, `odt`, `rtf`, `txt`, `xls`, `xlsx`, `ods`, `csv`, `ppt`, `pptx`, `odp`, `html`, `htm`, `epub`

---

## Instalación

### Dependencias del sistema

**Python 3**, **LibreOffice** (el motor de conversión) y **bubblewrap** (el aislamiento en el que se ejecuta):

```bash
# Arch / Manjaro
sudo pacman -S python libreoffice-still bubblewrap

# Debian / Ubuntu
sudo apt install python3 libreoffice bubblewrap
```

> No requiere ninguna librería externa de Python. Solo usa módulos de la biblioteca estándar.

`bubblewrap` no es opcional: sin él el programa se niega a convertir. Ver [Seguridad](#seguridad).

### Mover al PATH

Para poder llamarlo desde cualquier directorio sin escribir `python3` delante:

```bash
sudo cp pdfconversor.py /usr/local/bin/pdfconversor
sudo chmod +x /usr/local/bin/pdfconversor
```

O sin `sudo`, en tu directorio personal:

```bash
cp pdfconversor.py ~/.local/bin/pdfconversor
chmod +x ~/.local/bin/pdfconversor
```

---

## Uso

### Convertir un archivo

```bash
pdfconversor documento.docx
```

Con carpeta de destino:

```bash
pdfconversor documento.docx /ruta/destino
```

### Convertir una carpeta completa

```bash
pdfconversor -a
```

Especificando origen y destino:

```bash
pdfconversor -a /ruta/origen /ruta/destino
```

> Si la carpeta de destino no existe, se crea automáticamente.

### Ayuda y versión

```bash
pdfconversor -h        # o --help
pdfconversor -v        # o --version
pdfconversor -g        # o --guia (guía de uso completa, con ejemplos)
```

---

## Seguridad

Este programa existe para abrir documentos que te envían otras personas, así que
da por hecho que el archivo de entrada puede ser hostil.

**La conversión ocurre dentro de un sandbox sin acceso a red.** Un documento
puede llevar imágenes o enlaces a servidores externos, y LibreOffice los
descarga sin preguntar: eso revelaría a quien te envió el archivo cuándo lo
abriste y desde qué IP, y las peticiones saldrían desde tu equipo, alcanzando
direcciones que sólo tú ves. Dentro del sandbox no hay red que usar.

> **Efecto secundario:** si un documento legítimo enlaza una imagen remota,
> saldrá un hueco en su lugar. Es el precio de cerrar esa vía.

Además:

- LibreOffice se ejecuta con un **perfil desechable** y con el resto del sistema
  montado en **sólo lectura**, así que ningún documento deja rastro en tu
  configuración.
- Cada archivo tiene un **límite de 120 segundos** (ajustable con `TOPDF_TIMEOUT`).
  Un documento diseñado para atascar el analizador no bloquea el resto del lote.
- En modo `-a` se **omiten los enlaces simbólicos** que apunten fuera de la
  carpeta de origen, para que nadie pueda colar archivos ajenos en la salida.
- **Nunca se sobrescribe un PDF existente**: si el nombre está ocupado, el nuevo
  archivo se guarda como `documento-2.pdf`, `documento-3.pdf`, etc.
- El programa termina con **código distinto de cero** si alguna conversión falla.

### Pruebas de regresión

`test_seguridad.sh` reconstruye cada uno de estos ataques y comprueba que el
programa los rechaza:

```bash
./test_seguridad.sh
```
