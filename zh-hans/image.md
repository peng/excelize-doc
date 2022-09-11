# 图片

## 插入图片 {#AddPicture}

```go
func (f *File) AddPicture(sheet, cell, picture, format string) error
```

根据给定的工作表名称、单元格坐标、图片地址和图片格式（例如偏移、缩放和打印设置等），在对应的单元格上插入图片。此功能是并发安全的。

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
    // 插入图片
    if err := f.AddPicture("Sheet1", "A2", "image.jpg", ""); err != nil {
        fmt.Println(err)
    }
    // 插入带有缩放比例和超链接的图片
    if err := f.AddPicture("Sheet1", "D2", "image.png", `{
        "x_scale": 0.5,
        "y_scale": 0.5,
        "hyperlink": "#Sheet2!D8",
        "hyperlink_type": "Location"
    }`); err != nil {
        fmt.Println(err)
    }
    // 插入图片，并设置图片的外部超链接、打印和位置属性
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

可选参数 `autofit` 指定是否使图片尺寸自动适合单元格，其默认值为 `false`。

可选参数 `hyperlink` 用以指定图片的超链接。

可选参数 `hyperlink_type` 指定图片超链接的类型，支持外部链接 `External` 和内部链接 `Location` 两种类型，当使用 `Location` 连接到单元格位置时，坐标需要以 `#` 开始。

可选参数 `positioning` 定义了 Excel 电子表格中图片位置属性支持 `oneCell`（大小固定，位置随单元格改变）和 `absolute` （大小、位置均固定）两种类型，当不设置此参数时，默认属性为大小、位置随单元格而改变。

可选参数 `print_obj` 指定打印工作表时是否打印图片，其默认值为 `true`。

可选参数 `lock_aspect_ratio` 指定是否锁定图片的纵横比，其默认值为 `false。`

可选参数 `locked` 指定是否锁定图片。除非工作表受到保护，否则锁定对象无效。

可选参数 `x_offset` 指定图片与插入单元格的水平偏移量，其默认值为 0。

可选参数 `x_scale` 指定图片的水平缩放比例，默认值为 1.0，表示 100%。

可选参数 `y_offset` 指定图片与插入单元格的垂直偏移量，其默认值为 0。

可选参数 `y_scale` 指定图片的垂直缩放比例，其默认值为 1.0，表示 100%。

```go
func (f *File) AddPictureFromBytes(sheet, cell, format, name, extension string, file []byte) error
```

根据给定的工作表名称、单元格坐标、图片地址和图片格式（例如偏移、缩放和打印设置等）、图片描述、图片扩展名和 `[]byte` 类型的图片内容，在对应的单元格上插入图片。

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

## 获取图片 {#GetPicture}

```go
func (f *File) GetPicture(sheet, cell string) (string, []byte, error)
```

根据给定的工作表名称和单元格坐标获取工作簿上的图片，将以 `[]byte` 类型返回嵌入在 Excel 文档中的图片。此功能是并发安全的。该函数暂不支持获取通过 Kingsoft WPS&trade; 应用程序添加的单元格图片。例如，获取名为 `Sheet1` 的工作表上 `A2` 单元格上的图片：

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

## 删除图片 {#DeletePicture}

```go
func (f *File) DeletePicture(sheet, cell string) (err error)
```

根据给定的工作表名称和单元格坐标，删除对应单元格上的图片。
