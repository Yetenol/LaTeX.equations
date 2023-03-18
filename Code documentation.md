# Dependencies
| Package       | Usage                                            | Used macros            |
| ------------- | ------------------------------------------------ | ---------------------- |
| amsmath       | basic math components                            | ...                    |
| amssymb       | QED square symbol                                | `\BOX`                 |
| IEEEtrantools | align equations and their comments               | `\begin{IEEEeqnarray}` |
| array         | draw a vertical line next to the comments        | `\begin{array}`        |
| siunitx       | display units in comments                        | `\unit{}`              |
| etoolbox      | modify existing environments from other packages |
| xifthen       | detect empty arguments                           |

# Naming convention
| Macro type         | Naming convention                 | Example              |
| ------------------ | --------------------------------- | -------------------- |
| User marco         | `\⟨all lower case⟩`               | `\comment`         |
| Internal macro     | `\@⟨Upper Camel Case⟩` | `\@ClearDescription` |
| Internal variables | `\@⟨lower Camel Case⟩` | `\@commentWidth`                     |

# User macros

Public functions are accessible everywhere within the project.

## equations environment

Create a calculation block containing equations
```latex
\begin{equations}
``` 
```latex
\begin{equations}[⟨column setup⟩]
```
- by default, aligns one equation sign
- **@star variant**: enable citation numbers by default
- **@param** #1 ⟨column setup⟩: columns and their types used to display all equations

## setcommentwidth

Setter for `\@commentwidth`
```latex
\setcommentwidth{⟨comment width⟩}
```
- **@param** #1 ⟨comment width⟩: width of the column that displays the equation comments or units

# Environment specific user macros

Methods are functions which are only accessible inside the `{equations}` environment.

## intertext

Divide the equation block in two and write a conventional text paragraph between them
```latex
\intertext{⟨paragraph⟩}
```
- call it after the `\\` of the last equation without any further commands
- don't put a `\\` in front of the next equation
- **@param** #1 ⟨paragraph⟩: Regular text to display (no math mode)

## comment

Write a regular paragraph into the description column
```latex
\comment{⟨annotation text⟩}
```
- separate the comment with a vertical line and some horizontal space
- comments have a fixed width
- **@param** #1 ⟨annotation text⟩: Regular text to display (no math mode)

Implementation

Require the `{equations}` environment:  
- The method `\comment` is defined at the beginning of every `{equations}` environment.  
- It calls the private function `\@SetComment` to fulfill its functionality.

Print the text at the end of the line:  
- Call the private function `\@RenderDescription`

## equnit

Write a unit into the description column
```latex
\equnit{⟨unit expression⟩}
```
- separate the unit with horizontal space
- **@param** #1 ⟨unit expression⟩: A math expression or macros from the `siunitx` dependency

Implementation

Require the `{equations}` environment:  
- The method `\commentunit` is defined at the beginning of every `{equations}` environment.  
- It calls the private function `\@SetCommentUnit` to fulfill its functionality.

Print the text at the end of the line:  
- Call the private function `\@RenderDescription`

## qed

Write a square at the right end of the line
```latex
\qed
```
- call it after a linebreak `\\` at the end of the environment

Implementation

Require the `{equations}` environment:  
- The method `\qed` is defined at the beginning of every `{equations}` environment.  
- It calls the private function `\@Qed` to fulfill its functionality.

## label

Attach a node for cross-referencing e.g. `\eqref{eq:EXAMPLE}`
```latex
\label{⟨name⟩}
```
- ensure a citation number for the current line e.g. `(3)`
- **@param** #1 ⟨name⟩: new node for cross-referencing e.g. `\label{eq:EXAMPLE}`

Implementation

Modify functionality if called within the `{equations}` environment:  
- The method `\label` is replaces at the beginning of every `{equations}` environment.  
- It calls the private function `\@YesNumber` to fulfill its functionality.

Create and render an equation number:  
- Call `\IEEEyesnumber` from the `IEEEtrantools` dependency

Create a node for cross-referencing:  
- Call the built-in function `\label{⟨name⟩}`.

## sublabel

Attach a node for cross-referencing
```latex
\sublabel{⟨name⟩}
```
- ensure a citation number for the current line with letter e.g. `(3a)`

Implementation

Require the `{equations}` environment:  
- The method `\sublabel` is defined at the beginning of every `{equations}` environment.  
- It calls the internal macro `\@YesSubnumber` to fulfill its functionality.

Create an equation sub-number:  
- Call `\IEEEyessubnumber` from the `IEEEtrantools` dependency

Create a node for cross-referencing:  
- Call the built-in function `\label{⟨name⟩}`.

Let `\yessubnumber` be `\IEEEyessubnumber`

# Internal macros

Private functions are only accessible inside packages. Their name contains an `@` character. 

## ClearDescription

Clear the `\@description` variable to undefined
```
\@ClearDescription
```

## RenderDescription

Render a description paragraph in the comment column
```
\@RenderDescription
```
- only runs if a comment or commentunit has been set for the current line

## ThrowUnlessMath

Print math expression if in math mode
```
\@ThrowUnlessMath{⟨math expression⟩}{⟨macro name⟩}
```
- **@param** #1 ⟨math expression⟩: expression to be rendered
- **@param** #2 ⟨macro name⟩: macro name for error message
- **@throws** ERROR if in text mode

# Internal variables

Attributes are variables which are only accessible inside packages.

## description

Store the current line's comment or commentunit
```
\@description
```

## defaultColumnsSetup

Stores the default columns setup
```
defaultColumnsSetup
```
Default is `rCl`:
- a right-aligned expression,  
- an equals sign and  
- a left-aligned expression

## commentWidth

Store the width of the description column
```
\@commentWidth
```