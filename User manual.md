# Equations environment

Create a calculation with standardized alignment, comments, units, equation numbers and descriptions.
```latex
\begin{equations}
\end{equations}
``` 
- by default, aligns one equation sign
- **star variant**: enable citation numbers by default
- **⟨column setup⟩**: columns and their types used to display all equations

## Customize alignment

List LaTeX `columntypes` to align multiple equation lines
```latex
\begin{equations}[⟨column setup⟩]
```
- Example: `\begin{equations}[rCll]`

## Modify comment width

By default, comments take up one third of the writable page width.  
This width can be modified inside the environment for the current environment or outside for multiple.
```latex
\setcommentwidth{⟨comment width⟩}
```
- **⟨comment width⟩**: width of the column that displays the equation comments or units

# Environment specific user macros

The following macros are only available inside the `equations` environment.

## Insert text between equation lines

Divide the equation block in two and write a conventional text paragraph between them.
```latex
\intertext{⟨paragraph⟩}
```
- **⟨paragraph⟩**: Regular text to display (no math mode)
- call it after the `\\` of the last equation without any further commands
- don't put a `\\` in front of the next equation
- Example:
```latex
\begin{equations}
    U_1 &=& R_1 \cdot I_1 \\
\intertext{Analog ergibt sich:}
    U_2 &=& R_2 \cdot I_2 
\end{equations}
```

## Annotate an equation

Annotate the current equation line with wrapping text.
```latex
\comment{⟨annotation text⟩}
```
- **⟨annotation text⟩**: Regular text to display (no math mode)
- the annotation gets separated with a vertical line and some horizontal space
- if the allowed width gets exceeded, the text wraps

## Specify the unit of an equation

Note the unit for the terms of the current equation.
```latex
\equnit{⟨unit expression⟩}
```
- **⟨unit expression⟩**: A math expression or macros from the `siunitx` dependency
- the unit is surrounded by brackets and gets rendered at the right side of the page
- some horizontal space separates it from the equation

## Quod erat demonstrandum

Finish a mathematical proof.
```latex
\qed
```
- render a square on the right side of the page
- call it after a linebreak `\\` at the end of the environment

## Name and cross-reference an equation

Give the current equation a name to be able to reference it. An equation number is displayed automatically.
```latex
\label{⟨name⟩}
```
```latex
\sublabel{⟨name⟩}
```
- attaches a node for cross-referencing
- automatically create a citation number for the current line
- **⟨name⟩**: new node for cross-referencing e.g. `\label{eq:EXAMPLE}`
- `\label` renders as `(3)` and `\sublabel` renders as `(3a)`
- Examples:

Equation
```latex
\begin{equations}
    \Phi &=& MI
    \label{eq:magneticFlux}
\end{equations}
```

Reference
```latex
We have demonstrated this with equation \eqref{eq:magneticFlux}.
```