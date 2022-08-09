<h1> LaTeX equations package </h1>

Beatify multiple equations with comments and alignment.

Table of Contents
- [Installation](#installation)
- [Usage](#usage)
- [Notes](#notes)
- [Documentation](#documentation)
  - [Dependencies](#dependencies)
- [Public functions](#public-functions)
  - [newmathsymbol](#newmathsymbol)
  - [renewmathsymbol](#renewmathsymbol)
  - [providemathsymbol](#providemathsymbol)
  - [renewmathsymbol](#renewmathsymbol-1)
  - [equations environment](#equations-environment)
  - [setcommentwidth](#setcommentwidth)
- [Environment methods](#environment-methods)
  - [intertext](#intertext)
  - [comment](#comment)
  - [commentunit](#commentunit)
  - [qed](#qed)
  - [label](#label)
  - [sublabel](#sublabel)
- [Private functions](#private-functions)
  - [cleardescription](#cleardescription)
  - [renderdescription](#renderdescription)
  - [throwunlessmath](#throwunlessmath)
- [Private attributes](#private-attributes)
  - [description](#description)
  - [columnsalignoneequalsign](#columnsalignoneequalsign)
  - [commentwidth](#commentwidth)

# Installation

- add file **from External URL**  
    Create a `./sty` subfolder and add the external file `equations.sty` using the following url.  
    
    Example: This is supported in Overleaf.
    ```
    https://raw.githubusercontent.com/Yetenol/LaTeX.equations/main/equations.sty
    ```

- add file **manually**  
    Create a `./sty` subfolder and add the file [`equations.sty`](https://raw.githubusercontent.com/Yetenol/LaTeX.equations/main/equations.sty)

# Usage

- **Use package** in the preamble
    ```latex
    \usepackage{sty/equations}
    ```

# Notes

atBeginEnvironment
\newcommand{\comment}{\@setcomment}

outside:
\newcommand{\@setcomment}

delete eqnref
rename pasteDescription to renderDescriptionColumn

# Documentation

| General           | .                     |
| ----------------- | --------------------- |
| TeX Format        | LaTeX2e               |
| Required location | `./sty/equations.sty` |

## Dependencies
| Package       | Usage                                            | Used macros            |
| ------------- | ------------------------------------------------ | ---------------------- |
| amsmath       | basic math components                            | ...                    |
| amssymb       | QED square symbol                                | `\BOX`                 |
| IEEEtrantools | align equations and their comments               | `\begin{IEEEeqnarray}` |
| array         | draw a vertical line next to the comments        | `\begin{array}`        |
| siunitx       | display units in comments                        | `\unit{}`              |
| etoolbox      | modify existing environments from other packages |
| xifthen       | detect empty arguments                           |

# Public functions
Public functions are accessible everywhere within the project.

## newmathsymbol
```latex
\newmathsymbol{\⟨macro name⟩}{⟨math expression⟩}
```
- Create a custom snippet to use math expressions repeatedly
- **@param** #1 ⟨macro name⟩: name of the mathematical symbol or variable
- **@param** #2 ⟨math expression⟩: mathematical term that is printed
- **@throws** ERROR if ⟨macro name⟩ has already been defined

## renewmathsymbol
```latex
\renewmathsymbol{\⟨macro name⟩}{⟨math expression⟩}
```
- Analog to \newmathsymbol
- **@throws** ERROR if ⟨macro name⟩ has not previously been defined

## providemathsymbol
```latex
\providemathsymbol{\⟨macro name⟩}{⟨math expression⟩}
```
- Analog to \newmathsymbol
- creates a new definition for ⟨macro name⟩ only if one has not already been given

## renewmathsymbol
```latex
\renewmathsymbol{\⟨macro name⟩}{⟨math expression⟩}
```
- will always create the new definition, irrespective of any existing ⟨macro name⟩ with the same name
- this should be used sparingly.

## equations environment
```latex
\begin{equations}
``` 
or  
```latex
\begin{equations}[⟨column setup⟩]
```
- beatify multiple centered equations with comments, alignment and numbers
- allows only aligning one equation sign
- **@star variant**: enable citation numbers by default
- **@param** #1 ⟨column setup⟩: columns and their types used to display all equations

## setcommentwidth
```latex
\setcommentwidth{⟨comment width⟩}
```
- setter for \@commentwidth
- **@param** #1 ⟨comment width⟩: width of the column that displays the equation comments

# Environment methods
Methods are functions which are only accessible inside the `{equations}` environment.

## intertext
```latex
\intertext{⟨text⟩}
```
- write a regular paragraph across the entire page between two equations
- call it after the `\\` of the last equation without any further commands
- don't put a `\\` in front of the next equation
- **@param** #1 ⟨text⟩: Regular text to display (no math mode)

## comment
```latex
\comment{⟨text⟩}
```
- write a regular paragraph into the description column
- seperate the comment with a vertical line and some horizontal space
- comments have a fixed width
- call it as the last command in a row and after two &
- **@param**  #1 ⟨text⟩: Regular text to display (no math mode)
- The environment requirement is implemented by the method `\comment` calling the internal function `\@setcomment`.

## commentunit
```latex
\commentunit{⟨text⟩}
```
- write a unit into the description column
- seperate the unit with horizontal space
- **@param**  #1    Units to display non-italics (yes math mode)

Implementation
- Require the `{equations}` environment:  
    The method `\commentunit` is defined at the beginning of every `{equations}` environment.  
    It calls the internal function `\@setcommentunit` to fulfill its functionality.
- Print the text at the end of the line:  
    Call the private function `\@pasteDescription`


## qed
```latex
\qed
```
- render a _quod erum demonstrandum_ represented by a square at the end of the row
- call it after the \\ of the last equation
- don't call other command in the same row

Implementation
- Require the `{equations}` environment:  
    The method `\qed` is defined at the beginning of every `{equations}` environment.  
    It calls the internal function `\@qed` to fulfill its functionality.

## label
```latex
\label{⟨name⟩}
```
- attach a node for cross-referencing e.g. \eqnref{eqn:EXAMPLE}
- ensure a citation number for the current line e.g. (3)
- **@param** #1 ⟨name⟩: new node for cross-referencing e.g. \label{eqn:EXAMPLE}

Implementation
- modify the built-in function `\label`
- Create an equation number:  
    Call `\yesnumber` from the `IEEEtrantools` dependency

## sublabel
```latex
\sublabel{⟨name⟩}
```
- attach a node for cross-referencing
- ensure a citation number for the current line with letter e.g. (3a)

Implementation
- Create a node for cross-referencing:  
    It calls the built-in function `\label`.
- Create an equation sub-number:  
    Call `\yessubnumber` from the `IEEEtrantools` dependency
- Alias `\yessubnumber` to `\IEEEyessubnumber` from the `IEEEtrantools` dependency

# Private functions
Private functions are only accessible inside packages. Their name contains an `@` character. 

## cleardescription
> `\@cleardescription`
- clear the `description` variable to undefined

## renderdescription
> `\@pasteDescription}`
- render a description paragraph in the comment column
- only runs if a comment or commentunit has been set for the current line

## throwunlessmath
> `\@throwunlessmath{⟨math expression⟩}{⟨macro name⟩}`
- print math expression if in math mode
- **@param** #1 ⟨math expression⟩: expression to be rendered
- **@param** #2 ⟨macro name⟩: macro name for error message
- **@throws** ERROR if in text mode

# Private attributes
Attributes are variables which are only accessible inside packages.

## description
> `\@description`
- stores the current line's comment or commentunit

## defaultcolumnssetup
> `\@defaultcolumnssetup`
- stores the default columns setup `rCl`:  
    a right-aligned expression,  
    an equals sign and  
    a left-aligned expression

## commentwidth
> `\@commentwidth`
- stores the width of the description column