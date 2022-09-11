# Bild

## Bild hinzufügen {#AddPicture}

```go
func (f *File) AddPicture(sheet, cell, picture, format string) error
```

AddPicture bietet die Methode zum Hinzufügen eines Bilds zu einem Arbeitsblatt anhand eines bestimmten Bildformatsatzes (z. B. Versatz, Skalierung, Seitenverhältniseinstellung und Druckeinstellungen) und des Dateipfads. Diese Funktion wird für die gleichzeitige Verwendung unterstützt.

Zum Beispiel:

```go
package main

import (
    "fmt"
    _ "image/gif"
    _ "image/jpeg"
    _ "image/png"

    "github.com/xuri/excelize/v2"
)

func main() {
    f, err := excelize.OpenFile("Book1.xlsx")
    if err != nil {
        fmt.Println(err)
        return
    }
    // Fügen Sie ein Bild ein.
    if err := f.AddPicture("Sheet1", "A2", "image.png", ""); err != nil {
        fmt.Println(err)
    }
    // Fügen Sie ein Bild mit Skalierung in ein Arbeitsblatt ein.
    if err := f.AddPicture("Sheet1", "D2", "image.jpg", `{
        "x_scale": 0.5,
        "y_scale": 0.5
    }`); err != nil {
        fmt.Println(err)
    }
    // Fügen Sie mit Druckunterstützung einen Bildversatz in die Zelle ein.
    if err := f.AddPicture("Sheet1", "H2", "image.gif", `{
        "x_offset": 15,
        "y_offset": 10,
        "print_obj": true,
        "lock_aspect_ratio": false,
        "locked": false
    }`); err != nil {
        fmt.Println(err)
    }
    // Speichern Sie die Tabellenkalkulationsdatei mit dem Ursprungspfad.
    if err = f.Save(); err != nil {
        fmt.Println(err)
    }
    if err = f.Close(); err != nil {
        fmt.Println(err)
    }
}
```

Der optionale Parameter `autofit` gibt an, ob die Bildgröße automatisch in die Zelle passt, der Standardwert dafür ist `false`.

Der optionale Parameter `hyperlink` spezifiziert den Hyperlink des Bildes.

Der optionale Parameter `hyperlink_type` definiert zwei Arten von Hyperlinks `External` für die Website oder `Location` zum Verschieben in eine der Zellen in dieser Arbeitsmappe. Wenn der `hyperlink_type` `Location` ist, müssen die Koordinaten mit `#` beginnen.

Der optionale Parameter `positioning` definiert zwei Arten der Position eines Bildes in einer Excel-Tabelle, `oneCell` (Verschieben, aber nicht mit Zellen skalieren) oder `absolute` (Nicht verschieben oder mit Zellen skalieren). Wenn Sie diesen Parameter nicht festlegen, ist die Standardpositionierung Verschieben und Größe mit Zellen.

Der optionale Parameter `print_obj` gibt an, ob das Bild gedruckt wird, wenn das Arbeitsblatt gedruckt wird, der Standardwert dafür ist `true`.

Der optionale Parameter `lock_aspect_ratio` gibt an, ob das Seitenverhältnis für das Bild gesperrt ist, der Standardwert dafür ist `false`.

Der optionale Parameter `locked` gibt an, ob das Bild gesperrt ist. Das Sperren eines Objekts hat keine Auswirkung, es sei denn, das Blatt ist geschützt.

Der optionale Parameter `x_offset` gibt den horizontalen Versatz des Bildes mit der Zelle an, der Standardwert davon ist 0.

Der optionale Parameter `x_scale` spezifiziert die horizontale Skalierung von Bildern, der Standardwert davon ist 1.0, was 100% darstellt.

Der optionale Parameter `y_offset` gibt den vertikalen Versatz des Bildes mit der Zelle an, der Standardwert davon ist 0.

Der optionale Parameter `y_scale` spezifiziert die vertikale Skalierung von Bildern, der Standardwert davon ist 1.0, was 100% darstellt.

```go
func (f *File) AddPictureFromBytes(sheet, cell, format, name, extension string, file []byte) error
```

AddPictureFromBytes bietet die Methode zum Hinzufügen eines Bilds zu einem Blatt anhand eines bestimmten Bildformatsatzes (z. B. Versatz, Skalierung, Seitenverhältniseinstellung und Druckeinstellungen), einer alternativen Textbeschreibung, eines Erweiterungsnamens und eines Dateiinhalts im Typ `[]byte`.

Zum Beispiel:

```go
package main

import (
    _ "image/jpeg"
    "io/ioutil"

    "github.com/xuri/excelize/v2"
)

func main() {
    f := excelize.NewFile()

    file, err := ioutil.ReadFile("image.jpg")
    if err != nil {
        fmt.Println(err)
    }
    if err := f.AddPictureFromBytes("Sheet1", "A2", "", "Excel Logo", ".jpg", file); err != nil {
        fmt.Println(err)
    }
    if err := f.SaveAs("Book1.xlsx"); err != nil {
        fmt.Println(err)
    }
}
```

## Holen Sie sich ein Bild {#GetPicture}

```go
func (f *File) GetPicture(sheet, cell string) (string, []byte, error)
```

GetPicture bietet eine Funktion, mit der der Name der Bildbasis und der Rohinhalt anhand des Arbeitsblatts und des Zellennamens in eine Tabelle eingebettet werden können. Diese Funktion wird für die gleichzeitige Verwendung unterstützt. Diese Funktion gibt den Dateinamen in der Tabelle und den Dateiinhalt als Datentypen `[]byte` zurück.

Zum Beispiel:

```go
f, err := excelize.OpenFile("Book1.xlsx")
if err != nil {
    fmt.Println(err)
    return
}
file, raw, err := f.GetPicture("Sheet1", "A2")
if err != nil {
    fmt.Println(err)
    return
}
if err := ioutil.WriteFile(file, raw, 0644); err != nil {
    fmt.Println(err)
}
```

## Bild löschen {#DeletePicture}

```go
func (f *File) DeletePicture(sheet, cell string) (err error)
```

DeletePicture bietet eine Funktion zum Löschen von Diagrammen in einer Tabelle anhand des angegebenen Arbeitsblatts und des Zellennamens. Beachten Sie, dass die Bilddatei derzeit nicht aus dem Dokument gelöscht wird.
