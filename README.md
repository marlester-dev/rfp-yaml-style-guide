# RFP YAML Style Guide

Version 1.0.0
Licensed under the [CC-By 4.0 License](https://creativecommons.org/licenses/by/4.0/).

## Table of Contents

1. [Introduction](#introduction)
2. [File basics](#file-basics)
   - [File name](#file-name)
   - [File encoding](#file-encoding)
   - [New line ending](#new-line-ending)
3. [File structure](#file-structure)
   - [Ordering of file contents](#ordering-of-file-contents)
4. [Formatting](#formatting)
   - [Indentation](#indentation)
   - [No excess whitespaces](#no-excess-whitespaces)
   - [Key naming conventions](#key-naming-conventions)
   - [Data types](#data-types)
     - [Booleans](#booleans)
     - [Lists](#lists)
     - [Mappings](#mappings)
     - [Strings](#strings)
       - [Multi-line strings](#multi-line-strings)
   - [Column limit](#column-limit)
5. [Null values](#null-values)
   - [Default values](#default-values)
6. [Comments](#comments)

## Introduction

This guide outlines our conventions for writing YAML™ (YAML Ain't Markup Language) files.  
Adhering to this guide ensures consistency, readability, and maintainability of code.  
Was primarily made for the ReallyFakePlayers project, hence the name.  

## File basics

### File name

Use the `.yml` extension only. The `.yaml` extension is not permitted.  
File names must follow the **kebab-case** format, ending with the `.yml` extension.

Good: `example-file.yml`  
Bad: `exampleFile.yaml`

### File encoding

File shall be encoded in **UTF-8**.

### New line ending

End each file with the new line.

```yaml
# Good
# (File)
key-one: "value"
key-two: "value"

# (The blank line at the end is not shown, but should be present.)

# Bad
# (File)
key-one: "value"
key-two: "value"
# (No blank line at the end.)
```

## File structure

### Ordering of file contents

The order you choose for contents of your file can have a great effect on learnability.
However, there's no single correct recipe for how to do it; different files may order their contents in different ways.

What is important is that each file uses ***some* logical order**, which its maintainer could explain if asked.
For example, new data structures are not just habitually added to the end of the file,
as that would yield "chronological by date added" ordering, which is not a logical ordering.

You can also choose to insert a blank line between logical sections of content to improve readability,
but it's entirely up to your preference.

```yaml
# Good
foo-number: 1
foo-style: 1
doo-number: 1
doo-style: 1

# Bad
foo-number: 1
doo-style: 1
foo-style: 1
doo-number: 1
```

## Formatting

### Indentation

Use soft-tabs with a two space indent.

```yaml
# Good
example:
  one: 1

# Bad
example:
    bad: 2
```

### No excess whitespaces

Don't put any excess (or trailing) whitespaces where they don't belong.

```yaml
# Good
example: 1

# Bad
example:  1  
```

### Key naming conventions

Key names must be written in **kebab-case**.

```yaml
# Good
example-key: 1

# Bad
exampleKey: 1
```

### Data types

#### Booleans

Only use `true` and `false` as boolean values, in lower case.  
That means you shall not use `TRUE`, `on`, `False`, `yes`, etc. as boolean values.

```yaml
# Good
one: true
two: false

# Bad
one: True
two: on
three: yes
```

#### Lists

Lists in YAML are also known as sequences or arrays.

Lists can be written in different styles, however, we only allow the use of block style lists.
Flow style (that looks like JSON) is not allowed.  
*Note: Empty [] lists are still allowed.*

Block style sequences need to be indented under the key they belong to.

```yaml
# Good
example:
  - 1
  - 2
  - 3

# Bad
example:
- 1
- 2
- 3

example: [1, 2, 3]
```

#### Mappings

Mappings in YAML are also known as associative arrays, hash tables,
key/value pairs, collections or dictionaries.

Mappings can be written in different styles, however, we only allow the use
of block style mappings. Flow style (that looks like JSON) is not allowed.

```yaml
# Good
example:
  one: 1
  two: 2

# Bad
example: { one: 1, two: 2 }
```

#### Strings

Strings are _preferably_ quoted with double quotes (`"`).

```yaml
# Good
example: "Hi there!"

# Bad
example: 'Hi there!'

# Bad
example: Hi there
```

##### Multi-line strings

Avoid the use of `\n` or other new line indicators in YAML configuration when
possible. The same applies to avoiding long, single line, strings.

Instead, make use of the literal style (preserves new lines) and folded style
(does not preserve new lines) strings.

```yaml
# Good
literal-example: |
  This example is an example of literal block scalar style in YAML.
  It allows you to split a string into multiple lines.
folded-example: >
  This example is an example of a folded block scalar style in YAML.
  It allows you to split a string into multi lines, however, it magically
  removes all the new lines placed in your YAML.

# Bad
literal-example: "This example is an example of literal block scalar style in YAML.\nIt allows you to split a string into multiple lines.\n"
folded-example-same-as: "This example is an example of a folded block scalar style in YAML. It allows you to split a string into multi lines, however, it magically removes all the new lines placed in your YAML.\n"
```

In the examples above the no chomping operators are used (`|`, `>`). This is
preferred, unless the example requires a different handling of the ending new
line. In those cases the use of the strip operator (`|-`, `>-`: no trailing new
line, any additional new lines are removed from the end) or keep operator
(`|+`, `>+`: trailing new line, and keep all additional new lines from the end)
is allowed.

### Column limit

Every line has a column limit of 80 characters. A "character" means any Unicode code point.
Except as noted below, any line that would exceed this limit must be shortened.

> Each Unicode code point counts as one character, even if its display width is greater or less.
> For example, if using fullwidth characters, you may choose to wrap the line earlier than where this rule strictly requires.

**Exceptions**:
1. Lines where obeying the column limit is *really* not possible.
2. Very long identifiers, on the rare occasions they are needed for, are allowed to exceed the column limit.

```yaml
# Good
example: >
  I am a very very very very very very very
  very very very very very very long string.

# Bad
example: "I am a very very very very very very very very very very very very very long string."
```

## Null values

Null values should be implicitly marked. The use of explicit null values must
be avoided (`~` and `null`).

```yaml
# Good
example:

# Bad
example: ~
example: null
```

### "Default" values

We use a special "default" values tactic to represent a "default" value.  
Add a comment saying the type of the value and the assumed default value if not set (null).  
You may do it not 100% like in the examples, but you got the point.

```yaml
# The example value should be an integer
# If not set, the default value is assumed to be a random number between 10 and 90
example:
```
*Note: When working with lists it's also good to support an empty list as a "not set" option, example:*
```yaml
# Either so:

# The example value should be a string list
# If not set, the default value is assumed to be a random words list
example:

# Or so:

# The example value should be a string list
# If not set, the default value is assumed to be a random words list
example: []
```

## Comments

Adding comments to blocks of YAML can really help the reader understand the
example better.

The indentation level of the comment must match the current indentation level. Preferably the comment is written above the line the comment applies to, otherwise lines
may become hard to read on smaller displays.

Comments shall start with a capital letter and have a space between the
comment hash `#` and the start of the comment.

```yaml
# Good
example:
  # Comment
  one: true

# Acceptable, but prefer the above
example:
  one: true # Comment

# Bad
example:
# Comment
  one: false
  #Comment
  two: false
  # comment
  three: false
```

[![CC-By 4.0 License](https://i.creativecommons.org/l/by/4.0/88x31.png)](https://creativecommons.org/licenses/by/4.0/)
