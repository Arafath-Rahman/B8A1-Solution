# TypeScript Under The Hood

<div style="text-align: justify;">As being a superset of Javascript, Typescript inherits a lot from Javascript. When we write javascript code, we can catch the errors only when we run the code (runtime), but in typescript we can catch the errors while we are writing the code (compile-time). To understand that, let’s see how typescript works under the hood.
<br>
<br>
When we create a (.)ts file and started writing code, the code editor (IDE) sends our code to the Typescript LSP (tsserver). Typescript’s Language Server Protocol (LSP) is the bridge between the IDE and the Typescript compiler (tsc). The LSP interacts with IDE in real time and provides feedbacks regarding our code writing, like autocompleting properties, providing errors for assigning wrong type. The compiler’s (tsc) main job is to compile the Typescript codes into Javascript so that the browser and node.js can understand. It completes the process into 4 different steps: 
<br>
<br>

**1. Scan & Parse:** When we write any code, a scanner scans the source code character by character and converts them into tokens (`Keywords`/`Identifiers`/`Operators`). A parser form a `Abstract Syntax Tree (AST)` with those tokens.

**2. Binding:** A binder traverses the tree and identifies all the variables & functions, interfaces & classes and maps them into a `Symbol Table`.

**3. Type Checking:** In this step, the type checker looks at the `tree` and the s`ymbol table` and checks if anything is against the rules. For example: if any variables has mismatched values or not, are the constraints properly maintained etc.

**4. Convert to JavaScript:** In the last step, an emmitter takes the tree and start translating into output files, leaving out carefully all the Typescript `types`, `type aliases` we have created, `interfaces` and `generics` we’ve built & added, and make it all pure Javascript code.

The LSP stays active in the first 3 parts, reads the tree, table and provides us valuable insights like: autocomplete suggestions, it queries and provides definition & references, perform renames, and also generate the red errors in our IDE when we type something wrong, and also provides explanation when we hover over them.

## The Role of `tsconfig.json`

In this compiling cycle `tsconfig.json` file plays a big role. It’s the rulebook for the `tsc` (compiling lifecycle). This file alsways stays in the root directory of a Typescript project.

```tsx
// structure of tsconfig.json file
{
  "compilerOptions": {
    // File Layout
    "rootDir": "./src",
    "outDir": "./dist",

    // Environment Settings
    "module": "esnext",
    "target": "esnext",
    "types": [],

    // Other Outputs
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true,

    // Stricter Typechecking Options
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true

    // Recommended Options
    "strict": true,
    "jsx": "react-jsx",
    "verbatimModuleSyntax": true,
    "isolatedModules": true,
    "noUncheckedSideEffectImports": true,
    "moduleDetection": "force",
    "skipLibCheck": true
  }
}

```

There are a lot of options we can set in the file, but we’ll be discussiing only the most used ones:

- Here we can define which files/folders we need to `include` and `exclude`. For example, we generally excludes the `node_modules` folder while we compile.
- We set many rules for the Compiler here. For example: we use `target` to set our modern code standards like `esnext` , `module` to `nodenext` to set the module system for modern node.js projects.
- We set the `strict` flag to `true` so that the wide range of type checking behaviours are maintained, and set `moduleDetection` to `force` so that the script stays moduler.
- The `outDir`, and `rootDir` flags help us to set the file layout, so that we can define where the code we write stays and where the compiled javacript codes can go.
- There are many other flags like `sourceMap`, `declaration`, and `declarationMap` that helps the code compiling process directly.

This file is the master command book for typescript. Without this the compiler won’t know where the files are , what rules it should follow, and what version of javascript code it should generate. So as a developer we have to stay mindful about this file before starting a typescript project. </div>
