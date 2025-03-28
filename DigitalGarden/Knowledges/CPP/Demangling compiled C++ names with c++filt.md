---
title: Demangling compiled C++ names with c++filt
source: https://www.ibm.com/docs/en/xl-c-and-cpp-aix/16.1?topic=names-demangling-compiled-c-cfilt
author: 
published: 2024-01-26
created: 2025-02-13
description: IBM Documentation.
tags:
  - clippings
---

The c++filt utility is a filter that copies characters from file names or standard input to standard output, replacing all mangled names with their corresponding demangled names. You can use the filter directly with file name arguments, and the filter outputs the demangled names of all mangled names in the files; or you can use a shell command that inputs text, such as specific mangled names, and pipe it to the filter, so that the filter provides the demangled names of the names you specified.

## c++filt command syntax![Read syntax diagram](https://www.ibm.com/docs/en/SSGH3R_16.1.0/c.gif)[![Skip visual syntax diagram](https://www.ibm.com/docs/en/SSGH3R_16.1.0/c.gif)](https://www.ibm.com/docs/en/xl-c-and-cpp-aix/?topic=names-demangling-compiled-c-cfilt#skip)

You can specify one or more of the following options:

\-m

Produces a symbol map, containing a side-by-side listing of demangled names in the left column and their corresponding mangled names in the right column.

\-s

Produces a continuous listing of each demangled name followed immediately by its mangled name.

\-w *width*

Prints demangled names in fields width characters wide. If the name is shorter than width, it is padded on the right with blanks; if longer, it is truncated to width.

\-C

Demangles standalone class names, such as `Q2_1X1Y`.

\-S

Demangles special compiler-generated symbol names, such as` __vft1X` (represents a virtual function).

*filename*

Is the name of the file containing the mangled names you want to demangle. You can specify more than one file name.

For example, the following command shows the symbols contained in an object file `functions.o`, producing a side-by-side listing of the mangled and demangled names with a field width of 40 characters:

```
c++filt -m -w 40 functions.o
```

The output is displayed as follows:

```
C++ Symbol Mapping
demangled:                              Mangled:

Average::insertValue(double)            insertValue__7AverageFd
Average::getCount()                     getCount__7AverageFv
Average::getTotal()                     getTotal__7AverageFv
Average::getAverage()                   getAverage__7AverageFv
```

The following command shows the demangled name immediately followed by the mangled name:

```
echo getAverage__7AverageFv | c++filt -s
```

The output is displayed as follows:

```
Average::getAverage()getAverage__7AverageFv
```