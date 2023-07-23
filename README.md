# Typst Renderer

Renders `typst` code blocks, and optionally math blocks, into images using [Typst](https://github.com/typst/typst) through the power of WASM! This is still very much in development, so suggestions and bugs are welcome!

## Small Things to NOTE
- This plugin uses Typst 0.6.0
- Typst does not currently support exporting to HTML only PDFs and PNGs. So due to image scaling, the rendered views may look a bit terrible. If you know how to fix this PLEASE HELP.
- File paths should be relative to the vault folder.
- System fonts are not loaded by default as this takes about 20 seconds (on my machine). There is an option in settings to enable them (requires a reload of the plugin).
- You can not import on mobile as file reading is NOT supported on mobile, this is due to `SharedArrayBuffer`s not being available on mobile but is available for some reason on desktop.

## Using Packages
The plugin supports only the reading of packages from the [`@preview`](https://github.com/typst/packages#downloads) and [`@local`](https://github.com/typst/packages#local-packages) namespaces. Please use the Typst cli to download your desired packages. This means the plugin accesses files outside of your vault but only to read them, it does not modify or create files outside of your vault.

## Math Block Usage
The plugin can render `typst` inside math blocks! By default this is off, to enable it set the "Override Math Blocks" setting or use the "Toggle math block override" command. Math block types are conserved between Obsidian and Typst, `$...$` -> `$...$` and `$$...$$` -> `$ ... $`.

From what I've experimented with, normal math blocks are okay with Typst but Typst is not happy with any Latex code.

For styling and using imports with math blocks see the next section.

## Preamables
Need to style your `typst` code the same way everytime and don't to write it out each time? Or using math blocks and need a way to import things? Use PREAMABLES! 

Preamables are prepended to your `typst` code before compiling. There are three different types in settings:
- `shared`: Prepended to all `typst` code.
- `math`: Prepended to `typst` code only in math blocks.
- `code`: Prepended to `typst` code only in code blocks.

## Known Issues
### Runtime Error Unreachable or Recursive Use Of Object
These occur when the Typst compiler panics for any reason and means the compiler cannot be used again until it is restarted. There should be more information in the console log so please create an issue with this error!

## Example

```
```typst
= Fibonacci sequence
The Fibonacci sequence is defined through the
_recurrence relation_ $F_n = F_(n-1) + F_(n-2)$.
It can also be expressed in closed form:

$ F_n = floor(1 / sqrt(5) phi.alt^n), quad
  phi.alt = (1 + sqrt(5)) / 2 $

#let count = 10
#let nums = range(1, count + 1)
#let fib(n) = (
  if n <= 2 { 1 }
  else { fib(n - 1) + fib(n - 2) }
)

The first #count numbers of the sequence are:

#align(center, table(
  columns: count,
  ..nums.map(n => $F_#n$),
  ..nums.map(n => str(fib(n))),
))

```​
```

<img src="assets/example.png">

## Installation
Install "Typst Renderer" from the community plugins tab in settings

or 

Install it by copying `main.js`, `styles.css`, and `manifest.json` from the releases tab to the folder `.obsidian/plugins/obsidian-typst` in your vault.

## TODO / Goals (In no particular order)
- [x] Better font loading
- [x] Fix importing
- [x] Fix Github Actions
- [ ] Better error handling
- [x]? Fix output image scaling
- [ ] Use HTML output
- [x] Override default equation rendering
- [ ] Custom editor for `.typ` files
- [ ] Mobile file reading
- [ ] Automate package downloading
