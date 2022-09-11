# 圖片

## 插入圖片 {#AddPicture}

```go
func (f *File) AddPicture(sheet, cell, picture, format string) error
```

根據給定的工作表名稱、儲存格坐標、圖片地址和圖片格式（例如偏移、縮放和打印設定等），在對應的儲存格上插入圖片。此功能是併發安全的。

例如：

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
    f := excelize.NewFile()
    // 插入圖片
    if err := f.AddPicture("Sheet1", "A2", "image.jpg", ""); err != nil {
        fmt.Println(err)
    }
    // 插入帶有縮放比例和超鏈接的圖片
    if err := f.AddPicture("Sheet1", "D2", "image.png", `{
        "x_scale": 0.5,
        "y_scale": 0.5,
        "hyperlink": "#Sheet2!D8",
        "hyperlink_type": "Location"
    }`); err != nil {
        fmt.Println(err)
    }
    // 插入圖片，並設定圖片的外部超鏈接、打印和位置屬性
    if err := f.AddPicture("Sheet1", "H2", "image.gif", `{
        "x_offset": 15,
        "y_offset": 10,
        "hyperlink": "https://github.com/xuri/excelize",
        "hyperlink_type": "External",
        "print_obj": true,
        "lock_aspect_ratio": false,
        "locked": false,
        "positioning": "oneCell"
    }`); err != nil {
        fmt.Println(err)
    }
    if err := f.SaveAs("Book1.xlsx"); err != nil {
        fmt.Println(err)
    }
}
```

可選參數 `autofit` 設定是否使圖片尺寸自動適合存儲格，其缺省值為 `false`。

可選參數 `hyperlink` 用以設定圖片的超鏈接。

可選參數 `hyperlink_type` 設定圖片超鏈接的類型，支持外部鏈接 `External` 和內部鏈接 `Location` 兩種類型，當使用 `Location` 連接到存儲格位置時，坐標需要以 `#` 開始。

可選參數 `positioning` 定義了 Excel 電子錶格中圖片位置屬性支持 `oneCell`（大小固定，位置隨存儲格改變）和 `absolute` （大小、位置均固定）兩種類型，當不設定此參數時，默認屬性為大小、位置隨存儲格而改變。

可選參數 `print_obj` 設定打印工作表時是否打印圖片，其缺省值為 `true`。

可選參數 `lock_aspect_ratio` 設定是否鎖定圖片的縱橫比，其缺省值為 `false。`

可選參數 `locked` 設定是否鎖定圖片。除非工作表受到保護，否則鎖定對象無效。

可選參數 `x_offset` 設定圖片與插入存儲格的水平偏移量，其缺省值為 0。

可選參數 `x_scale` 設定圖片的水平縮放比例，其缺省值為 1.0，表示 100%。

可選參數 `y_offset` 設定圖片與插入存儲格的垂直偏移量，缺省值為 0。

可選參數 `y_scale` 設定圖片的垂直縮放比例，其缺省值為 1.0，表示 100%。

```go
func (f *File) AddPictureFromBytes(sheet, cell, format, name, extension string, file []byte) error
```

根據給定的工作表名稱、儲存格坐標、圖片地址和圖片格式（例如偏移、縮放和打印設定等）、圖片描述、圖片擴展名和 `[]byte` 類別的圖片內容，在對應的儲存格上插入圖片。

例如：

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

## 獲取圖片 {#GetPicture}

```go
func (f *File) GetPicture(sheet, cell string) (string, []byte, error)
```

根據給定的工作表名稱和儲存格坐標獲取活頁簿上的圖片，將以 `[]byte` 類別傳回嵌入在 Excel 文檔中的圖片。此功能是併發安全的。該函數暫不支持獲取通過 Kingsoft WPS&trade; 應用程式添加的存儲格圖片。例如，獲取名為 `Sheet1` 的工作表上 `A2` 儲存格上的圖片：

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

## 刪除圖片 {#DeletePicture}

```go
func (f *File) DeletePicture(sheet, cell string) (err error)
```

根據給定的工作表名稱和儲存格坐標，刪除對應儲存格上的圖片。
