---
title: Validate - 内置的过滤与验证器
permalink: /docs/zh/validate-builtin-filters-validators
key:
---

**提示**

- `intX` 包含: `int`, `int8`, `int16`, `int32`, `int64`
- `uintX` 包含: `uint`, `uint8`, `uint16`, `uint32`, `uint64`
- `floatX` 包含: `float32`, `float64`

## 内置过滤器

> Filters powered by: [gookit/filter](https://github.com/gookit/filter)

过滤器/别名 | 描述信息 
-------------------|-------------------------------------------
`int`  | Convert value(string/intX/floatX) to `int` type `v.FilterRule("id", "int")`
`uint`  | Convert value(string/intX/floatX) to `uint` type `v.FilterRule("id", "uint")`
`int64`  | Convert value(string/intX/floatX) to `int64` type `v.FilterRule("id", "int64")`
`float`  | Convert value(string/intX/floatX) to `float` type
`bool`  | Convert string value to bool. (`true`: "1", "on", "yes", "true", `false`: "0", "off", "no", "false")
`trim/trimSpace`  | Clean up whitespace characters on both sides of the string
`ltrim/trimLeft`  | Clean up whitespace characters on left sides of the string
`rtrim/trimRight`  | Clean up whitespace characters on right sides of the string
`int/integer`  | Convert value(string/intX/floatX) to int type `v.FilterRule("id", "int")`
`lower/lowercase` | Convert string to lowercase
`upper/uppercase` | Convert string to uppercase
`lcFirst/lowerFirst` | Convert the first character of a string to lowercase
`ucFirst/upperFirst` | Convert the first character of a string to uppercase
`ucWord/upperWord` | Convert the first character of each word to uppercase
`camel/camelCase` | Convert string to camel naming style
`snake/snakeCase` | Convert string to snake naming style
`escapeJs/escapeJS` | Escape JS string.
`escapeHtml/escapeHTML` | Escape HTML string.
`str2ints/strToInts` | Convert string to int slice `[]int` 
`str2time/strToTime` | Convert date string to `time.Time`.
`str2arr/str2array/strToArray` | Convert string to string slice `[]string`

## 内置验证器

几大类别：

- 类型验证
- 大小、长度验证
- 字段值比较验证
- 上传文件验证
- 日期验证
- 字符串检查验证
- 其他验证

验证器/别名 | 描述信息
-------------------|-------------------------------------------
`required`  | 字段为必填项，值不能为空 
`-/safe`  | 标记当前字段是安全的，无需验证
`int/integer/isInt`  | 检查值是 `intX` `uintX` 类型
`uint/isUint`  |  检查值是 `uintX` 类型（`value >= 0`）
`bool/isBool`  |  检查值是布尔字符串(`true`: "1", "on", "yes", "true", `false`: "0", "off", "no", "false").
`string/isString`  |  检查值是字符串类型.
`float/isFloat`  |  检查值是 float(`floatX`) 类型
`slice/isSlice`  |  检查值是 slice 类型(`[]intX` `[]uintX` `[]byte` `[]string` 等).
`in/enum`  |  检查值是否在给定的枚举列表中
`notIn`  |  检查值不是在给定的枚举列表中
`contains`  |  检查输入值是否包含给定的值
`notContains`  |  检查输入值是否不包含给定值
`range/between`  |  检查值是否为数字且在给定范围内
`max/lte`  |  检查输入值小于或等于给定值
`min/gte`  |  检查输入值大于或等于给定值(for `intX` `uintX` `floatX`)
`eq/equal/isEqual`  |  检查输入值是否等于给定值
`ne/notEq/notEqual`  |  检查输入值是否不等于给定值
`lt/lessThan`  |  检查值小于给定大小(use for `intX` `uintX` `floatX`)
`gt/greaterThan`  |  检查值大于给定大小(use for `intX` `uintX` `floatX`)
`intEq/intEqual`  |  检查值为int且等于给定值
`len/length`  |  检查值长度等于给定大小(use for `string` `array` `slice` `map`).
`minLen/minLength`  |  检查值的最小长度是给定大小
`maxLen/maxLength`  |  检查值的最大长度是给定大小
`email/isEmail`  |   检查值是Email地址字符串
`regex/regexp`  |  检查该值是否可以通过正则验证
`arr/array/isArray`  |  检查值是数组`array`类型
`map/isMap`  |  检查值是MAP类型
`strings/isStrings`  |  检查值是字符串切片类型(`[]string`).
`ints/isInts`  |  检查值是int slice类型(only allow `[]int`).
`eqField`  |  检查字段值是否等于另一个字段的值
`neField`  |  检查字段值是否不等于另一个字段的值
`gtField`  |  检查字段值是否大于另一个字段的值
`gteField`  | 检查字段值是否大于或等于另一个字段的值
`ltField`  |  检查字段值是否小于另一个字段的值
`lteField`  |  检查字段值是否小于或等于另一个字段的值
`file/isFile`  |  验证是否是上传的文件
`image/isImage`  |  验证是否是上传的图片文件，支持后缀检查
`mime/mimeType/inMimeTypes`  |  验证是否是上传的文件，并且在指定的MIME类型中
`date/isDate` | 检查字段值是否为日期字符串。（只支持几种常用的格式） eg `2018-10-25`
`gtDate/afterDate` | 检查输入值是否大于给定的日期字符串
`ltDate/beforeDate` | 检查输入值是否小于给定的日期字符串
`gteDate/afterOrEqualDate` | 检查输入值是否大于或等于给定的日期字符串
`lteDate/beforeOrEqualDate` | 检查输入值是否小于或等于给定的日期字符串
`hasWhitespace` | 检查字符串值是否有空格
`ascii/ASCII/isASCII` | 检查值是ASCII字符串
`alpha/isAlpha` | 验证值是否仅包含字母字符
`alphaNum/isAlphaNum` | 验证是否仅包含字母、数字
`alphaDash/isAlphaDash` | 验证是否仅包含字母、数字、破折号（ - ）以及下划线（ _ ）
`multiByte/isMultiByte` | Check value is MultiByte string.
`base64/isBase64` | 检查值是Base64字符串
`dnsName/DNSName/isDNSName` | 检查值是DNS名称字符串
`dataURI/isDataURI` | Check value is DataURI string.
`empty/isEmpty` | Check value is Empty string.
`hexColor/isHexColor` | 检查值是16进制的颜色字符串
`hexadecimal/isHexadecimal` | 检查值是十六进制字符串
`json/JSON/isJSON` | 检查值是JSON字符串。
`lat/latitude/isLatitude` | 检查值是纬度坐标
`lon/longitude/isLongitude` | 检查值是经度坐标
`mac/isMAC` | 检查值是MAC字符串
`num/number/isNumber` | 检查值是数字字符串. `>= 0`
`printableASCII/isPrintableASCII` | Check value is PrintableASCII string.
`rgbColor/RGBColor/isRGBColor` | 检查值是RGB颜色字符串
`url/isURL` | 检查值是URL字符串
`ip/isIP`  |  检查值是IP（v4或v6）字符串
`ipv4/isIPv4`  |  检查值是IPv4字符串
`ipv6/isIPv6`  |  检查值是IPv6字符串
`CIDR/isCIDR` | 检查值是 CIDR 字符串
`CIDRv4/isCIDRv4` | 检查值是 CIDR v4 字符串
`CIDRv6/isCIDRv6` | 检查值是 CIDR v6 字符串
`uuid/isUUID` | 检查值是UUID字符串
`uuid3/isUUID3` | 检查值是UUID3字符串
`uuid4/isUUID4` | 检查值是UUID4字符串
`uuid5/isUUID5` | 检查值是UUID5字符串
`filePath/isFilePath` | 检查值是一个存在的文件路径
`unixPath/isUnixPath` | 检查值是Unix Path字符串
`winPath/isWinPath` | 检查值是Windows路径字符串
`isbn10/ISBN10/isISBN10` | 检查值是ISBN10字符串
`isbn13/ISBN13/isISBN13` | 检查值是ISBN13字符串
