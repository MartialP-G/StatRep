# StatRep — LaTeX Package for Reproducible SAS Documents

This repository contains the source of the **StatRep** LaTeX package.
The package is available at:

* GitHub: https://github.com/MartialP-G/StatRep
* CTAN: http://www.ctan.org/pkg/statrep
* SAS (original): http://support.sas.com/StatRepPackage — *obsolete*


# Overview

The **StatRep** system is an open-source package that works with SAS, LaTeX, 
and a suite of SAS macros to enable you to create dynamic documents with reproducible results. It combines the LaTeX markup language with your SAS code so that both the code and resulting SAS output are automatically displayed in your final PDF document.

With **StatRep**, your analysis is fully documented and reproducible from a single source--your LaTeX document. The system automatically produces a SAS program so that both the SAS output and the SAS code that produced the output are displayed.

The package provides two environments and two tags:

* Environments `Datastep` and `Sascode` display SAS code.
* Tags `Listing` and `Graphic` display SAS output.

## Workflow

1. Compile your `.tex` document with `pdflatex`. This generates two SAS
   programs:
   - `myarticle_SR.sas` — calls the preamble file, cleans output
     directories, then runs the aggregated SAS code from your LaTeX file.
   - `myarticle_SR_preamble.sas` — contains custom variables and a call
     to `statrep_macros.sas`.
2. Execute `myarticle_SR.sas` in SAS to produce the results.
3. Recompile with `pdflatex` to include the SAS results in your document.
   Repeat if necessary for correct framing of listings.

## SAS Path Options

The `SASmode` option controls how the current path is resolved:

* `SASmode=WorkStation` (default): uses `.` as the root directory.
* `SASmode=Server`: uses `&currentpath`, derived from the
  `&_SASPROGRAMFILE` SAS macro variable. Use this for SAS Studio,
  SAS OnDemand, or any server-based SAS environment.

## SAS Result Export Macros

Three macros in `statrep_macros.sas` allow you to export SAS results or
macro variable values directly into your LaTeX document via
`jobname_VarSAStoTex.tex`, which is loaded automatically at compile time:

* `%STATREPResults(variable, format)` — exports a single variable using
  the specified format.
* `%STATREPAllResults()` — processes all numeric variables in a dataset
  and generates their LaTeX definitions.
* `%STATREPMACROVAR(variable)` — exports a single SAS macro variable.

The generated file contains `\def` commands (e.g. `\def\PARAMETER{365}`)
that can be used directly in your LaTeX document as `\PARAMETER`.

## Character Encoding

The package automatically maps the LaTeX `inputenc` encoding to the
correct SAS output encoding (`UTF-8` or `WLATIN1`). If `inputenc` is not
loaded, `UTF-8` is used by default.

# Documentation

* *StatRep User's Guide* (`statrepmanual.pdf`) — installation and usage
* `quickstart.tex` — simple working example
* `statrep.pdf` — LaTeX implementation details

# License

    Copyright (c) 2015 SAS Institute Inc.
    Copyright (c) 2026 Martial Phélippé-Guinvarc'h

    This work may be distributed and/or modified under the conditions of
    the LaTeX Project Public License (LPPL), version 1.3c or later.
    See http://www.latex-project.org/lppl.txt for details.

    This work has the LPPL maintenance status 'maintained'.
    The Current Maintainer is Martial Phélippé-Guinvarc'h
    (Martial dot Phelippe-GuinvarcH at univ-lemans dot fr).

    The original version was developed by Tim Arnold (SAS Institute).
    This is an independent update and is not officially supported
    by SAS Institute Inc.

# Requirements

* `pdfLaTeX` typesetting engine 1.30 or later
* LaTeX packages: `verbatim`, `graphicx`, `xkeyval`, `calc`, `ifthen`,
  `etoolbox`. Included in standard distributions (TeXLive, MiKTeX).
* SAS 9.2 or later

## Documentation 

Documentation is included in the distribution, available at the links provided above.

  * *The StatRep User's Guide* (`statrepmanual.pdf`) includes installation instructions and details on how to use the system.
  * A simple example of using the package (`quickstart.tex`)
  * Documentation on the LaTeX implementation (`statrep.pdf`)

The generated SAS program includes calls to macros that use the SAS
Output Delivery System (ODS) document to capture the output as external files.
These SAS macros are included in this package (``statrep_macros.sas``).

Limited support is provided for SAS-generated LaTeX output, using a special tagset: you can generate the tagset with the included SAS program ``statrep_tagset.sas``. 


# Distribution Layout

## Files in this repository

    statrep.dtx           — package source (documentation + code)
    statrep.ins           — install file (for MiKTeX users)
    statrep_macros.sas    — SAS macro file
    statrep_tagset.sas    — SAS tagset and style for LaTeX tabular output
    README.md             — this file

## Generating the package files

Compile `statrep.dtx` with `pdflatex` to generate all derived files:

    pdflatex statrep.dtx

This produces:
* `statrep.sty` — the LaTeX package
* `longfigure.sty` — the longfigure package
* `statrep.cfg` — default configuration file (edit to set your SAS macro path)
* `statrep.pdf` — this documentation

## Distribution layout (statrep.zip)

For standard distribution, the zip file is structured as follows:

    README
    LICENSE
    statrep.dtx
    statrep.ins
    statrep.sty
    longfigure.sty
    statrep.cfg
    doc/
        quickstart.tex
        statrepmanual.tex
        statrepmanual.pdf
        statrep.pdf
    sas/
        statrep_macros.sas
        statrep_tagset.sas

## CTAN layout

CTAN generates the `*.sty` files from `statrep.dtx` automatically,
so they are omitted from the CTAN submission:

    README
    LICENSE
    statrep.dtx
    statrep.ins
    doc/
        quickstart.tex
        statrepmanual.tex
        statrepmanual.pdf
        statrep.pdf
    sas/
        statrep_macros.sas
        statrep_tagset.sas

