# Interactive Propositional Resolution Environment

A web-based educational environment for learning and practicing **propositional resolution through interactive proof construction**.

The application supports both automatic and learner-directed resolution. It is designed to provide immediate feedback during proof construction while keeping the individual reasoning steps visible and inspectable.

## Online Application

The application runs entirely in a web browser:

**https://tiveqq.github.io/resolution-method/static/index.html**

No installation is required to use the deployed version.

## Overview

Resolution is commonly introduced through manually constructed proofs. During such exercises, students must correctly transform formulas, identify resolvable clauses, apply the resolution rule, and keep track of the resulting derivation.

This project provides an interactive environment in which these steps can be explored with immediate validation.

The application combines two complementary workflows:

- **Automatic mode** generates a resolution derivation and allows the individual stages and resulting proof structure to be inspected.
- **Interactive mode** allows the learner to select parent clauses and construct a resolution proof incrementally, with each attempted step validated by the application.

The focus of the project is on the **design, functionality, and usability of the tool** as an environment for practicing propositional resolution.

## Features

### Formula input and validation

The application provides syntax-aware input for propositional formulas and supports both textual and mathematical notation for logical operators.

Supported concepts include:

- propositional variables;
- truth (`⊤`) and falsity (`⊥`);
- negation;
- conjunction;
- disjunction;
- implication;
- equivalence;
- sequents.

Input is parsed and validated before proof generation.

### CNF transformation

Formulas are transformed into **conjunctive normal form (CNF)** before resolution is applied.

The application exposes the transformation process so that the intermediate representation used by the resolution procedure can be inspected.

### Automatic resolution

In automatic mode, the application presents the validity-refutation workflow step by step:

1. negation of the input formula;
2. transformation into CNF;
3. extraction of clauses;
4. application of resolution;
5. construction of the resulting derivation.

### Interactive proof construction

Interactive mode allows learners to construct a resolution proof themselves.

Users can:

- select parent clauses;
- attempt an individual resolution step;
- receive immediate validation;
- inspect newly derived clauses;
- continue the proof incrementally;
- undo and redo accepted steps;
- inspect the history of the derivation.

This mode is intended to support guided trial-and-error exploration rather than simply displaying a completed proof.

### Resolution strategies

The application supports selectable resolution strategies, including:

- linear resolution;
- unit resolution.

These strategies influence the selection of clauses during proof construction and search.

### Proof visualization

Resolution derivations can be inspected as:

- a **proof tree**, or
- a **step table**.

The proof-tree visualization supports navigation of larger derivations and interactive node positioning.

### Export

Results can be exported in several formats:

- **DIMACS CNF** for clauses;
- **SVG** for proof-tree visualization;
- **LaTeX** for generated proof representations.

### Responsive interface

The interface is designed to adapt to both desktop and narrower screen layouts.

## Example Workflow

A typical workflow is:

```text
Propositional formula
        |
        v
Syntax validation
        |
        v
Formula representation
        |
        v
CNF transformation
        |
        v
Set of clauses
        |
        +-----------------------+
        |                       |
        v                       v
Automatic resolution     Interactive resolution
        |                 (learner-selected steps)
        |                       |
        +-----------+-----------+
                    |
                    v
             Resolution proof
                    |
              +-----+-----+
              |           |
              v           v
            Tree         Table
```

## Technology

The application is implemented as a client-side web application.

The main technologies used by the project include:

- **JavaScript** — logical processing and application logic;
- **ANTLR** — lexical and syntactic analysis of propositional formulas;
- **Monaco Editor** — syntax-aware formula input and diagnostics;
- **D3.js** — interactive proof-tree visualization.

The processing pipeline operates in the browser without requiring a server-side proof-processing stage.


## Development

The application is implemented as a client-side JavaScript project. Source
modules are bundled with **Webpack**, while the propositional-logic lexer and
parser are generated from an **ANTLR** grammar.

### Prerequisites

To build the project locally, install:

- [Node.js](https://nodejs.org/) and npm
- a modern web browser such as Chrome, Firefox, Safari, or Edge

Clone the repository and enter the project directory:

```bash
git clone https://github.com/tiveqq/resolution-method.git
cd resolution-method
```

Install the project dependencies defined in `package.json`:

```bash
npm install
```

### Project structure

The main parts of the project are organized as follows:

```text
resolution-method/
├── antlr-generated/
│   ├── PropositionalLogic.g4
│   ├── PropositionalLogicLexer.js
│   ├── PropositionalLogicParser.js
│   └── PropositionalLogicVisitor.js
├── src/
│   ├── change-language.js
│   ├── cnf-converter.js
│   ├── index.js
│   ├── monaco-editor.js
│   ├── proof-tree.js
│   ├── resolution-method.js
│   └── ui.js
├── static/
│   ├── index.html
│   └── style.css
└── webpack.config.js
```

The directories have the following roles:

- `src/` contains the main application logic, including formula processing,
  CNF conversion, resolution algorithms, proof visualization, Monaco Editor
  integration, and user-interface logic.
- `antlr-generated/` contains the propositional-logic grammar and the lexer,
  parser, and visitor generated by ANTLR.
- `static/` contains the browser entry point and application styles.
- `webpack.config.js` defines how the JavaScript modules are bundled for use
  in the browser.

### Building the application

Webpack bundles the application modules into the JavaScript file used by the
browser.

After installing the dependencies, run:

```bash
npx webpack
```

Webpack reads `webpack.config.js` and generates the bundled output used by
`static/index.html`.

If Webpack is installed globally, the equivalent command is:

```bash
webpack
```

### Running locally

After building the project, serve the repository using a local HTTP server.
For example, with Python:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000/static/index.html
```

Using a local HTTP server is recommended instead of opening `index.html`
directly from the filesystem, because the application loads JavaScript
modules and other browser resources.

### Regenerating the ANTLR parser

The propositional-logic grammar is defined in:

```text
antlr-generated/PropositionalLogic.g4
```

The repository already contains the generated lexer, parser, and visitor, so
ANTLR regeneration is **not required for a normal build**.

Regenerate these files only when the grammar is modified.

Install the ANTLR JavaScript tooling if needed:

```bash
npm install antlr4
```

Then regenerate the JavaScript parser from the grammar using ANTLR with the
JavaScript target.

For example, from the directory containing
`PropositionalLogic.g4`:

```bash
antlr4 -Dlanguage=JavaScript -visitor PropositionalLogic.g4
```

This regenerates the parser-related files used by the application, including
the lexer, parser, and visitor.

After changing the grammar, rebuild the browser bundle:

```bash
npx webpack
```

### Development workflow

For most changes to the application, the development cycle is:

```text
Edit source files in src/
        |
        v
Run npx webpack
        |
        v
Serve the repository locally
        |
        v
Open static/index.html in a browser
        |
        v
Test the application
```

When `PropositionalLogic.g4` is changed, regenerate the ANTLR files before
running Webpack.

### Main source modules

The application is divided into several modules:

- `cnf-converter.js` — formula representation, CNF conversion, simplification,
  and clause extraction.
- `resolution-method.js` — resolution rule and resolution strategies.
- `index.js` — main application logic, input processing, proof generation,
  interactive resolution, undo/redo, DIMACS handling, and explanations.
- `proof-tree.js` — proof-tree construction, D3.js visualization, SVG export,
  and LaTeX/TikZ generation.
- `monaco-editor.js` — Monaco Editor initialization, syntax assistance, and
  parser diagnostics.
- `ui.js` — dynamic interface elements, resolution tables, clause selection,
  and LaTeX output.
- `change-language.js` — interface localization.

## License

This project is released under the **MIT License**.

See [LICENSE](LICENSE) for details.
