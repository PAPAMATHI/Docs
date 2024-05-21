---
title: "PANDOC"
geometry: margin=2cm
output: pdf_document
---
# INSTALLATION GUIDE
##  Download latest padoc .deb file version:
-  WITH BROWZER
```bash
https://github.com/jgm/pandoc/releases
```

OR

-  WITH WGET
```bash
sudo wget "yoururl"
```

## INSTALLATION
use this commande line to install .deb file :
```bash
sudo dpkg -i pandoc-yourversion-amd64.deb
```
## INSTALL EXTERNAL PACKAGE
For runing correctly Pandoc we must download "textlive-full"

Run this following command :
```bash
sudo apt-get install texlive-full
```

# How to change informations files :
For modify author, title, date and document geometry, we must add this following paragraph :
```markdown
---
title: "Habits"
author: John Doe
date: March 22, 2005
geometry: margin=2cm
output: pdf_document
---
```

# Convert .md to .pdf
Run this following command :
```bash
pandoc yourfile.md -s -o yourfile.pdf
```
