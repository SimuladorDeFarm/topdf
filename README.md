# pdfconversor

Script de terminal para convertir documentos a PDF usando LibreOffice headless. Soporta conversión de archivos individuales o carpetas completas.

**Formatos soportados:** `doc`, `docx`, `odt`, `rtf`, `txt`, `xls`, `xlsx`, `ods`, `csv`, `ppt`, `pptx`, `odp`, `html`, `htm`, `epub`

---

## Instalación

### Dependencias del sistema

**Python 3** y **LibreOffice** (el motor de conversión):

```bash
# Arch / Manjaro
sudo pacman -S python libreoffice-still

# Debian / Ubuntu
sudo apt install python3 libreoffice
```

> No requiere ninguna librería externa de Python. Solo usa módulos de la biblioteca estándar.

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
