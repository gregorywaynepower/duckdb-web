---
layout: docu
redirect_from:
- docs/archive/1.1/test/functions/blob
title: Blob Functions
---

<!-- markdownlint-disable MD001 -->

This section describes functions and operators for examining and manipulating [`BLOB` values]({% link docs/1.1/sql/data_types/blob.md %}).

<!-- Start of section generated by scripts/generate_sql_function_docs.py -->
<!-- markdownlint-disable MD056 -->

| Name | Description |
|:--|:-------|
| [`blob || blob`](#blob--blob) | `BLOB` concatenation. |
| [`base64(blob)`](#base64blob) | Converts a `blob` to a base64 encoded `string`. |
| [`decode(blob)`](#decodeblob) | Converts `blob` to `VARCHAR`. Fails if `blob` is not valid UTF-8. |
| [`encode(string)`](#encodestring) | Converts the `string` to `BLOB`. Converts UTF-8 characters into literal encoding. |
| [`from_base64(string)`](#from_base64string) | Converts a base64 encoded `string` to a character string (`BLOB`). |
| [`from_binary(value)`](#from_binaryvalue) | Converts a value from binary representation to a blob. |
| [`from_hex(value)`](#from_hexvalue) | Converts a value from hexadecimal representation to a blob. |
| [`hex(blob)`](#hexblob) | Converts `blob` to `VARCHAR` using hexadecimal encoding. |
| [`octet_length(blob)`](#octet_lengthblob) | Number of bytes in `blob`. |
| [`read_blob(source)`](#read_blobsource) | Returns the content from `source` (a filename, a list of filenames, or a glob pattern) as a `BLOB`. See the [`read_blob` guide]({% link docs/stable/guides/file_formats/read_file.md %}#read_blob) for more details. |
| [`to_base64(blob)`](#to_base64blob) | Converts a `blob` to a base64 encoded `string`. |
| [`to_hex(blob)`](#to_hexblob) | Converts `blob` to `VARCHAR` using hexadecimal encoding. |
| [`unbin(value)`](#unbinvalue) | Converts a value from binary representation to a blob. |
| [`unhex(value)`](#unhexvalue) | Converts a value from hexadecimal representation to a blob. |

<!-- markdownlint-enable MD056 -->

#### `blob || blob`

<div class="nostroke_table"></div>

| **Description** | `BLOB` concatenation. |
| **Example** | `'\xAA'::BLOB || '\xBB'::BLOB` |
| **Result** | `\xAA\xBB` |

#### `base64(blob)`

<div class="nostroke_table"></div>

| **Description** | Converts a `blob` to a base64 encoded `string`. |
| **Example** | `base64('A'::BLOB)` |
| **Result** | `QQ==` |

#### `decode(blob)`

<div class="nostroke_table"></div>

| **Description** | Converts `blob` to `VARCHAR`. Fails if `blob` is not valid UTF-8. |
| **Example** | `decode('\xC3\xBC'::BLOB)` |
| **Result** | `ü` |

#### `encode(string)`

<div class="nostroke_table"></div>

| **Description** | Converts the `string` to `BLOB`. Converts UTF-8 characters into literal encoding. |
| **Example** | `encode('my_string_with_ü')` |
| **Result** | `my_string_with_\xC3\xBC` |

#### `from_base64(string)`

<div class="nostroke_table"></div>

| **Description** | Converts a base64 encoded `string` to a character string (`BLOB`). |
| **Example** | `from_base64('QQ==')` |
| **Result** | `A` |

#### `from_binary(value)`

<div class="nostroke_table"></div>

| **Description** | Converts a value from binary representation to a blob. |
| **Example** | `unbin('0110')` |
| **Result** | `\x06` |

#### `from_hex(value)`

<div class="nostroke_table"></div>

| **Description** | Converts a value from hexadecimal representation to a blob. |
| **Example** | `unhex('2A')` |
| **Result** | `*` |

#### `hex(blob)`

<div class="nostroke_table"></div>

| **Description** | Converts `blob` to `VARCHAR` using hexadecimal encoding. |
| **Example** | `hex('\xAA\xBB'::BLOB)` |
| **Result** | `AABB` |

#### `octet_length(blob)`

<div class="nostroke_table"></div>

| **Description** | Number of bytes in `blob`. |
| **Example** | `octet_length('\xAA\xBB'::BLOB)` |
| **Result** | `2` |

#### `read_blob(source)`

<div class="nostroke_table"></div>

| **Description** | Returns the content from `source` (a filename, a list of filenames, or a glob pattern) as a `BLOB`. See the [`read_blob` guide]({% link docs/1.1/guides/file_formats/read_file.md %}#read_blob) for more details. |
| **Example** | `read_blob('hello.bin')` |
| **Result** | `hello\x0A` |

#### `to_base64(blob)`

<div class="nostroke_table"></div>

| **Description** | Converts a `blob` to a base64 encoded `string`. |
| **Example** | `base64('A'::BLOB)` |
| **Result** | `QQ==` |

#### `to_hex(blob)`

<div class="nostroke_table"></div>

| **Description** | Converts `blob` to `VARCHAR` using hexadecimal encoding. |
| **Example** | `hex('\xAA\xBB'::BLOB)` |
| **Result** | `AABB` |

#### `unbin(value)`

<div class="nostroke_table"></div>

| **Description** | Converts a value from binary representation to a blob. |
| **Example** | `unbin('0110')` |
| **Result** | `\x06` |

#### `unhex(value)`

<div class="nostroke_table"></div>

| **Description** | Converts a value from hexadecimal representation to a blob. |
| **Example** | `unhex('2A')` |
| **Result** | `*` |

<!-- End of section generated by scripts/generate_sql_function_docs.py -->
