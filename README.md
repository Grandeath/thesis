# Praca Inżynierska

## 1. Instalacja LaTeX'a

```bash
sudo apt update
sudo apt install texlive-full latexmk biber make
```

## 2. Fonty (Times New Roman / Arial dla XeLaTeX)

```bash
sudo apt install ttf-mscorefonts-installer
```

Jeśli fonty kiedyś znikną to można zmienić w `main.tex` np. na `Liberation Serif`, `Liberation Sans`.

## 3. VS Code + LaTeX Workshop

### 3.1. Instalacja VS Code

```bash
sudo snap install code --classic
```

### 3.2. Instalacja rozszerzenia LaTeX

W VS Code:

1. Otwórz **Extensions** (`Ctrl+Shift+X`).
2. Wyszukaj: `LaTeX Workshop`.
3. Zainstaluj rozszerzenie: **LaTeX Workshop (James Yu)**
   (ID: `James-Yu.latex-workshop`).

## 4. Konfiguracja VS Code dla LaTeX-a

W folderze `thesis/` utwórz katalog `.vscode/` i plik `.vscode/settings.json`:

```json
{
  "latex-workshop.latex.recipes": [
    {
      "name": "XeLaTeX + biber (x2)",
      "tools": [
        "xelatex",
        "biber",
        "xelatex",
        "xelatex"
      ]
    }
  ],
  "latex-workshop.latex.tools": [
    {
      "name": "xelatex",
      "command": "xelatex",
      "args": [
        "-synctex=1",
        "-interaction=nonstopmode",
        "-file-line-error",
        "-output-directory=%OUTDIR%",
        "%DOC%"
      ]
    },
    {
      "name": "biber",
      "command": "biber",
      "args": [
        "--output-directory=%OUTDIR%",
        "%DOCFILE%"
      ]
    }
  ],
  "latex-workshop.latex.recipe.default": "XeLaTeX + biber (x2)",
  "latex-workshop.latex.autoBuild.run": "onSave",
  "latex-workshop.latex.outDir": "./build",
  "latex-workshop.view.pdf.viewer": "tab"
}
```

To nam daje kolejno:

* kompilację `xelatex -> biber -> xelatex -> xelatex`
* wrzucanie wszystkich wyników (PDF + `aux` itd.) do `./build`,
* automatyczny build po `Ctrl+S`,
* podgląd PDF w zakładce w VS Code.

## 5. Otwieranie projektu i kompilacja

1. Otwórz VS Code.
2. **File -> Open Folder…** i wskaż `thesis/`.
3. Otwórz `main.tex`.
4. Po lewej kliknij ikonę **TEX** (panel LaTeX Workshop).
5. Upewnij się, że recipe `XeLaTeX + biber (x2)` jest wybrane.
6. Teraz:

   * `Ctrl+S` -> zapis + automatyczna kompilacja do `build/main.pdf`,
   * w panelu LaTeX Workshop kliknij "View LaTeX PDF", żeby otworzyć podgląd.

## 6. SyncTeX (skok z `.tex` do PDF)

### 6.1. Użycie z Command Palette (działa zawsze)

1. Ustaw kursor w `*.tex` w miejscu, które chcesz zobaczyć w PDF.
2. `Ctrl+Shift+P`.
3. Wpisz: `SyncTeX from cursor`.
4. Wybierz: **LaTeX Workshop: SyncTeX from cursor**.

PDF przeskoczy we właściwe miejsce.

### 6.2. Własny skrót do SyncTeX (np. `Ctrl+Alt+S`)

1. `Ctrl+K`, potem `Ctrl+S` – otwiera **Keyboard Shortcuts**.
2. Wyszukaj: `SyncTeX from cursor`.
3. Przy komendzie **LaTeX Workshop: SyncTeX from cursor** kliknij w kolumnę z klawiszami.
4. Wciśnij np. `Ctrl+Alt+S`, zatwierdź.

Od teraz:

* `Ctrl+S` -> zapis + build,
* `Ctrl+Alt+S` -> SyncTeX z kursora do PDF (PDF skacze w miejsce, gdzie piszesz).

## 7. Ręczna kompilacja z terminala (jako awaryjny backup)

Będąc w katalogu `thesis/`:

```bash
# pełna sekwencja XeLaTeX + Biber (wyniki w ./build)
xelatex -synctex=1 -interaction=nonstopmode -file-line-error -output-directory=build main.tex
biber --output-directory=build main
xelatex -synctex=1 -interaction=nonstopmode -file-line-error -output-directory=build main.tex
xelatex -synctex=1 -interaction=nonstopmode -file-line-error -output-directory=build main.tex
```

I wtedy gotowy PDF: `thesis/build/main.pdf`.
