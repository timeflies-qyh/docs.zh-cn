---
title: System.Text.Json 中的 DateTime 和 DateTimeOffset 支持
description: 介绍如何在 System.web 库中支持 DateTime 和 DateTimeOffset 类型。
ms.technology: dotnet-standard
author: layomia
ms.author: laakinri
ms.date: 07/22/2019
helpviewer_keywords:
- JSON, Serializer, Utf8
- JSON DateTime, JSON DateTimeOffset
- DateTime, DateTimeOffset
- JsonSerializer, Utf8JsonReader, Utf8JsonWriter, JsonElement, JsonDocument
- JSON Serializer, JSON Reader, JSON Writer
- Converter, JSON Converter, DateTime Converter
- ISO, ISO 8601, ISO 8601-1:2019
ms.openlocfilehash: 8198359e2c54c4ed098703fbcc070f7469b3362a
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75344657"
---
# <a name="datetime-and-datetimeoffset-support-in-systemtextjson"></a>System.Text.Json 中的 DateTime 和 DateTimeOffset 支持

根据 ISO 8601:-2019 扩展配置文件，System.object 库分析并写入 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 值。
[转换器](xref:System.Text.Json.Serialization.JsonConverter%601)为序列化和反序列化提供自定义支持 <xref:System.Text.Json.JsonSerializer>。
使用 <xref:System.Text.Json.Utf8JsonReader> 和 <xref:System.Text.Json.Utf8JsonWriter>时，还可以实现自定义支持。

## <a name="support-for-the-iso-8601-12019-format"></a>支持 ISO 8601-1:2019 格式

<xref:System.Text.Json.JsonSerializer>、<xref:System.Text.Json.Utf8JsonReader>、<xref:System.Text.Json.Utf8JsonWriter>和 <xref:System.Text.Json.JsonElement> 类型根据 ISO 8601-1:2019 格式的扩展配置文件分析并写入 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 文本表示形式;例如，2019-07-26T16：59： 57-05：00。

可以用 <xref:System.Text.Json.JsonSerializer>序列化 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 数据：

[!code-csharp[example-serializing-with-jsonserializer](~/samples/snippets/standard/datetime/json/csharp/serializing-with-jsonserializer/Program.cs)]

还可以通过 <xref:System.Text.Json.JsonSerializer>反序列化它们：

[!code-csharp[example-deserializing-with-jsonserializer-valid](~/samples/snippets/standard/datetime/json/csharp/deserializing-with-jsonserializer-valid/Program.cs)]

对于默认选项，输入 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 文本表示形式必须符合扩展 ISO 8601-1:2019 配置文件。
尝试对不符合配置文件的表示形式进行反序列化将导致 <xref:System.Text.Json.JsonSerializer> 引发 <xref:System.Text.Json.JsonException>：

[!code-csharp[example-deserializing-with-jsonserializer-error](~/samples/snippets/standard/datetime/json/csharp/deserializing-with-jsonserializer-error/Program.cs)]

<xref:System.Text.Json.JsonDocument> 提供对 JSON 有效负载内容的结构化访问，包括 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 表示形式。 下面的示例演示了当给定温度集合时，可以计算星期一的平均温度：

[!code-csharp[example-computing-with-jsondocument-valid](~/samples/snippets/standard/datetime/json/csharp/computing-with-jsondocument-valid/Program.cs)]

尝试使用不符合的 <xref:System.DateTime> 表示法来计算平均温度将导致 <xref:System.Text.Json.JsonDocument> 引发 <xref:System.FormatException>：

[!code-csharp[example-computing-with-jsondocument-error](~/samples/snippets/standard/datetime/json/csharp/computing-with-jsondocument-error/Program.cs)]

较低级别 <xref:System.Text.Json.Utf8JsonWriter> 写入 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 数据：

[!code-csharp[example-writing-with-utf8jsonwriter](~/samples/snippets/standard/datetime/json/csharp/writing-with-utf8jsonwriter/Program.cs)]

<xref:System.Text.Json.Utf8JsonReader> 分析 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 数据：

[!code-csharp[example-reading-with-utf8jsonreader-valid](~/samples/snippets/standard/datetime/json/csharp/reading-with-utf8jsonreader-valid/Program.cs)]

尝试使用 <xref:System.Text.Json.Utf8JsonReader> 读取不符合格式将导致它引发 <xref:System.FormatException>：

[!code-csharp[example-reading-with-utf8jsonreader-error](~/samples/snippets/standard/datetime/json/csharp/reading-with-utf8jsonreader-error/Program.cs)]

## <a name="custom-support-for-xrefsystemdatetime-and-xrefsystemdatetimeoffset"></a><xref:System.DateTime> 和 <xref:System.DateTimeOffset> 的自定义支持

### <a name="when-using-xrefsystemtextjsonjsonserializer"></a>使用时 <xref:System.Text.Json.JsonSerializer>

如果希望序列化程序执行自定义分析或格式设置，可以实现[自定义转换器](xref:System.Text.Json.Serialization.JsonConverter%601)。
下面是几个示例：

#### <a name="using-datetimeoffsetparse-and-datetimeoffsettostring"></a>使用 `DateTime(Offset).Parse` 和 `DateTime(Offset).ToString`

如果无法确定输入 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 文本表示形式的格式，则可以在转换器读取逻辑中使用 `DateTime(Offset).Parse` 方法。 这允许使用。NET 对分析各种 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 文本格式的广泛支持，包括非 ISO 8601 字符串和不符合扩展 ISO 8601-1:2019 配置文件的 ISO 8601 格式。 与使用序列化程序的本机实现相比，此方法的性能大大降低。

对于序列化，可以在转换器写入逻辑中使用 `DateTime(Offset).ToString` 方法。 这允许您使用任何[标准日期和时间格式](../base-types/standard-date-and-time-format-strings.md)以及[自定义日期和时间格式](../base-types/custom-date-and-time-format-strings.md)编写 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 值。
与使用序列化程序的本机实现相比，这种性能也明显降低。

[!code-csharp[example-showing-datetime-parse](~/samples/snippets/standard/datetime/json/csharp/datetime-converter-examples/example1/Program.cs)]

> [!NOTE]
> 当实现 <xref:System.Text.Json.Serialization.JsonConverter%601>且 `T` <xref:System.DateTime>时，`typeToConvert` 参数将始终为 `typeof(DateTime)`。
参数可用于处理多态事例和使用泛型以高性能方式获取 `typeof(T)`。

#### <a name="using-xrefsystembufferstextutf8parser-and-xrefsystembufferstextutf8formatter"></a>使用 <xref:System.Buffers.Text.Utf8Parser> 和 <xref:System.Buffers.Text.Utf8Formatter>

如果输入 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 文本表示形式符合 "R"、"l"、"O"、"G" 标准日期和时间格式字符串中的一个或 "G"[标准日期和时间格式字符串](../base-types/standard-date-and-time-format-strings.md)，或者需要根据这些格式中的一种进行写入，则可以在转换器逻辑中使用快速的基于 utf-8 的分析和格式设置方法。 这比使用 `DateTime(Offset).Parse` 和 `DateTime(Offset).ToString`快得多。

此示例演示一个自定义转换器，该转换器根据["R" 标准格式](../base-types/standard-date-and-time-format-strings.md#the-rfc1123-r-r-format-specifier)对 <xref:System.DateTime> 值进行序列化和反序列化：

[!code-csharp[example-showing-utf8-parser-and-formatter](~/samples/snippets/standard/datetime/json/csharp/datetime-converter-examples/example2/Program.cs)]

> [!NOTE]
> "R" 标准格式的长度始终为29个字符。

#### <a name="using-datetimeoffsetparse-as-a-fallback-to-the-serializers-native-parsing"></a>使用 `DateTime(Offset).Parse` 作为序列化程序的本机解析的回退

如果你通常希望输入 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 数据符合扩展 ISO 8601-1:2019 配置文件，则可以使用序列化程序的本机分析逻辑。 您还可以实现一种回退机制。
此示例显示，在无法使用 <xref:System.Text.Json.Utf8JsonReader.TryGetDateTime(System.DateTime@)>分析 <xref:System.DateTime> 的文本表示形式后，转换器将使用 <xref:System.DateTime.Parse(System.String)>成功分析数据。

[!code-csharp[example-showing-datetime-parse-as-fallback](~/samples/snippets/standard/datetime/json/csharp/datetime-converter-examples/example3/Program.cs)]

### <a name="when-writing-with-xrefsystemtextjsonutf8jsonwriter"></a>写入时 <xref:System.Text.Json.Utf8JsonWriter>

如果要使用 <xref:System.Text.Json.Utf8JsonWriter>编写自定义 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 文本表示形式，则可以将自定义表示形式设置为 <xref:System.String>、`ReadOnlySpan<Byte>`、`ReadOnlySpan<Char>`或 <xref:System.Text.Json.JsonEncodedText>，然后将其传递给相应的 <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue%2A?displayProperty=nameWithType> 或 <xref:System.Text.Json.Utf8JsonWriter.WriteString%2A?displayProperty=nameWithType> 方法。

下面的示例演示如何使用 <xref:System.DateTime.ToString(System.String,System.IFormatProvider)>创建自定义 <xref:System.DateTime> 格式，然后使用 <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue(System.String)> 方法编写该格式：

[!code-csharp[example-custom-writing-with-utf8jsonwriter](~/samples/snippets/standard/datetime/json/csharp/custom-writing-with-utf8jsonwriter/Program.cs)]

### <a name="when-reading-with-xrefsystemtextjsonutf8jsonreader"></a>在读取时 <xref:System.Text.Json.Utf8JsonReader>

如果要使用 <xref:System.Text.Json.Utf8JsonReader>读取自定义 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 文本表示形式，则可以使用 <xref:System.Text.Json.Utf8JsonReader.GetString>获取当前 JSON 令牌的值作为 <xref:System.String>，然后使用自定义逻辑分析该值。

下面的示例演示如何使用 <xref:System.Text.Json.Utf8JsonReader.GetString>检索自定义 <xref:System.DateTimeOffset> 文本表示形式，然后使用 <xref:System.DateTimeOffset.ParseExact(System.String,System.String,System.IFormatProvider)>进行分析：

[!code-csharp[example-custom-reading-with-utf8jsonreader](~/samples/snippets/standard/datetime/json/csharp/custom-reading-with-utf8jsonreader/Program.cs)]

## <a name="the-extended-iso-8601-12019-profile-in-systemtextjson"></a>系统中的扩展 ISO 8601-1:2019 配置文件

### <a name="date-and-time-components"></a>日期和时间组件

<xref:System.Text.Json> 中实现的扩展 ISO 8601-1:2019 配置文件定义了日期和时间表示的以下组件。 这些组件用于定义分析和格式化 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 表示形式时支持的各种粒度级别。

| 组件       | 格式                      | 描述                                                                     |
|-----------------|-----------------------------|---------------------------------------------------------------------------------|
| 年            | “yyyy”                      | 0001-9999                                                                       |
| 月           | “MM”                        | 01-12                                                                           |
| 日             | “dd”                        | 01-28、01-29、01-30、01-31 （基于月份/年份）                                  |
| 小时            | “HH”                        | 00-23                                                                           |
| 分钟          | “mm”                        | 00-59                                                                           |
| 秒          | “ss”                        | 00-59                                                                           |
| 第二部分 | “FFFFFFF”                   | 至少一个数字，最多为16位                                      |
| 时间偏移     | “K”                         | "Z" 或 "（" + "/"-"） HH"： "mm"                                                |
| 部分时间    | "HH"： "mm"： "ss [FFFFFFF]"     | 不带 UTC 偏移信息的时间                                             |
| 完整日期       | "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-yyyy'-'mm'-'dd"            | 日历日期                                                                   |
| 全部时间       | "' Partial time'K"           | 一天中的 UTC 或本地时间与本地时间和 UTC 之间的时间偏移量 |
| 日期时间       | "" 完整时间 "不是" "完整时间" " | 日历日期和当天的时间，例如 2019-07-26T16：59： 57-05：00                   |

### <a name="support-for-parsing"></a>支持分析

定义以下级别的粒度：

1. "Full date"
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-yyyy'-'mm'-'dd"

2. "" 完整日期 "不是" "小时" "：" 分钟 "
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"

3. "" 完整日期 "不是" "部分时间" "
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss" （可[排序（"s"）格式说明符](../base-types/standard-date-and-time-format-strings.md#the-sortable-s-format-specifier)）
    2. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFF

4. "" 完整日期 "不是" "，时间小时" "：" "分钟" "时间偏移" "
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "mmZ"
    2. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "mm （' + '/'-'） HH '： ' mm"

5. "日期时间"
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ssZ"
    2. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFFZ"
    3. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh '： ' MM '： ' ss （' + '/'-'） HH '： ' MM"
    4. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFF （"+"/"-"） HH "：" mm "

如果有秒的小数部分，则必须至少有一个数字;不允许 `2019-07-26T00:00:00.`。
尽管允许最多包含16个小数位，但仅分析前7个小数。 超出的任何内容都将被视为零。
例如，`2019-07-26T00:00:00.1234567890` 将按 `2019-07-26T00:00:00.1234567`进行分析。
这是为了保持与仅限此分辨率的 <xref:System.DateTime> 实现兼容。

不支持闰秒。

### <a name="support-for-formatting"></a>支持格式设置

为格式化定义了下列粒度级别：

1. "" 完整日期 "不是" "部分时间" "
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss" （可[排序（"s"）格式说明符](../base-types/standard-date-and-time-format-strings.md#the-sortable-s-format-specifier)）

        用于设置不包含秒的小数部分和不含偏移信息的 <xref:System.DateTime> 的格式。

    2. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFF

        用于设置 <xref:System.DateTime> 的小数部分的格式，但不含偏移信息。

2. "日期时间"
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ssZ"

        用于将 <xref:System.DateTime> 的格式设置为不包含秒的小数部分，但使用 UTC 偏移量。

    2. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFFZ"

        用于将 <xref:System.DateTime> 的格式设置为包含秒的小数部分和 UTC 偏移量。

    3. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh '： ' MM '： ' ss （' + '/'-'） HH '： ' MM"

        用于将 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 的格式设置为不包含秒的小数部分，但使用本地偏移量。

    4. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFF （"+"/"-"） HH "：" mm "

        用于将 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 的小数部分格式设置为小数部分和本地偏移量。

如果存在，则最多可以写入7个小数位。 这与 <xref:System.DateTime> 实现保持一致，此方法仅限于此解决方案。
