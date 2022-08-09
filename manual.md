<h1> Code documentation </h1>

[⌂](README.md) ›

Table of Contents
- [User macros](#user-macros)
  - [newmathsymbol](#newmathsymbol)
  - [renewmathsymbol](#renewmathsymbol)
  - [providemathsymbol](#providemathsymbol)
  - [renewmathsymbol](#renewmathsymbol-1)
  - [equations environment](#equations-environment)
  - [setcommentwidth](#setcommentwidth)
- [Environment specific user macros](#environment-specific-user-macros)
  - [intertext](#intertext)
  - [comment](#comment)
  - [commentunit](#commentunit)
  - [qed](#qed)
  - [label](#label)
  - [sublabel](#sublabel)

# User macros
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

# Environment specific user macros
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

Implementation
- Require the `{equations}` environment:  
    The method `\comment` is defined at the beginning of every `{equations}` environment.  
    It calls the private function `\@SetComment` to fulfill its functionality.
- Print the text at the end of the line:  
    Call the private function `\@RenderDescription`

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
    It calls the private function `\@SetCommentUnit` to fulfill its functionality.
- Print the text at the end of the line:  
    Call the private function `\@RenderDescription`


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
    It calls the private function `\@Qed` to fulfill its functionality.

## label
```latex
\label{⟨name⟩}
```
- attach a node for cross-referencing e.g. `\eqref{eq:EXAMPLE}`
- ensure a citation number for the current line e.g. `(3)`
- **@param** #1 ⟨name⟩: new node for cross-referencing e.g. `\label{eq:EXAMPLE}`

Implementation
- Modify functionality if called within the `{equations}` environment:  
    The method `\label` is replaces at the beginning of every `{equations}` environment.  
    It calls the private function `\@YesNumber` to fulfill its functionality.
- Create and render an equation number:  
    Call `\IEEEyesnumber` from the `IEEEtrantools` dependency
- Create a node for cross-referencing:  
    Call the built-in function `\label{⟨name⟩}`.

## sublabel
```latex
\sublabel{⟨name⟩}
```
- attach a node for cross-referencing
- ensure a citation number for the current line with letter e.g. `(3a)`

Implementation
- Require the `{equations}` environment:  
    The method `\sublabel` is defined at the beginning of every `{equations}` environment.  
    It calls the internal macro `\@YesSubnumber` to fulfill its functionality.
- Create an equation sub-number:  
    Call `\IEEEyessubnumber` from the `IEEEtrantools` dependency
- Create a node for cross-referencing:  
    Call the built-in function `\label{⟨name⟩}`.
- Let `\yessubnumber` be `\IEEEyessubnumber`