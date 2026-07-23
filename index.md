# Dev Notes & Learning Plan

## General Notes

- Actually _writing_ pseudocode is an important step that I shouldn't skip. This helps to breakdown and simplify problems.
  - When writing pseudocode, a useful quick gut-check question set for when I catch myself writing "and":
    1. Could I write a unit test for just the first half that would still make sense on its own?
    2. Is there any plausible future caller who wants only one side?
    3. Does the intermediate result (after step 1, before step 2) have a meaningful name/type?
- Essentially, computer programming is about taking some input and creating some output. What happens in between the input and output, is what we could call a black box.
  ![Black Box diagram](./imgs/blackbox.svg)
- An _algorithm_ is just a step-by-step set of instructions to solve a problem.
- The basic building blocks of programming:
  - variables
  - data types (strings, integers, floats, etc.)
  - data structures (arrays, objects, tuples, etc.)
  - conditionals
  - boolean expressions
  - loops
  - functions
- Most code is built out of three foundational pillars:
  | Pillar | Purpose | Key Examples |
  | --------------- | ------------------------------------ | ------------------------------------------------ |
  | Data | Storing and categorizing information | Variables, Primitive Data Types, Data Structures |
  | Logic & Control | Directing the flow of execution | Conditionals, Loops, Boolean Expressions |
  | Abstraction | Bundling and reusing code | Functions, Modules, Classes |

## Detailed Dev Notes

- [Command Line / Terminal](./dev-notes/command-line.md)
- [JavaScript](./dev-notes/javascript.md)
- [React](./dev-notes/react/index.md)
- [Testing](./dev-notes/testing.md)

## Learning Plan

- [Learning Plan](./learning-plan/index.md)

## Markdown Resource

<a href="https://www.markdownguide.org/basic-syntax/" target="_blank">Markdown Guide Basic Syntax</a>

## Unicode

To switch between the US keyboard and the Unicode Hex Input keyboard `Ctrl` + `Spacebar`

To use the hex codes in the links below hold `Opt` and type the 4 digit code (e.g. `2500`)

<a href="https://en.wikipedia.org/wiki/List_of_Unicode_characters" target="_blank">Wikipedia List of Unicode Characters</a><br>
<a href="https://en.wikipedia.org/wiki/List_of_Unicode_characters#Box_Drawing" target="_blank">Box Drawing Unicode Characters</a>
