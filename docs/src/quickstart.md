# [Quick Start](@id quickstart)

This section describes how you can set up your demos easily in four lines of codes_.

## Manage your demo files

The rules of demo management are super simple:

* you need one demo page to hold all the demo files
* a [demo page](@ref concepts_page) has several [demo sections](@ref concepts_section)
* a demo section either
    * has other demo sections as nested subsections, or,
    * has the [demo files](@ref concepts_card).

In the following example:

* we have one demo page: "simplest_demopage"
* "simplest_demopage" has one demo section: "dependencies"
* "dependencies" has two demo subsections: "part1" and "part2"
* "part1" and "part2" holds all the demo files
* "assets" are ignored by `DemoCards.jl`

```text
../demos/simplest_demopage
└── dependencies
    ├── part1
    │   ├── assets
    │   ├── simple_documenter.md
    │   ├── simple_julia.md
    │   └── simple_literate.md
    └── part2
        ├── assets
        ├── simple_fileio.md
        └── simple_images.md
```

```@setup simplest_demopage
using DemoCards
using DemoCards: DemoPage
root = "../demos/simplest_demopage"
```

```@repl simplest_demopage
DemoPage(root)
```

## Deploy your demo page

Deployment is also very simple:

1. load a theme using `cardtheme`
2. create a demopage using `makedemos`
3. pass the results to `Documenter.jl`

```julia
theme = cardtheme()
demopage = makedemos("demos/simplest_demopage") # relative path to docs/

format = Documenter.HTML(edit_branch = "master",
                         assets = [theme, ])
makedocs(format = format,
         pages = [
            "Home" => "index.md",
            "Examples" => demopage,
         ],
         sitename = "Awesome demos")
```

!!! info

    🚧 `cardtheme` is currrently at its draft stage. It simply copies an
    existing css file and returns you a path to it.

After it's set up, you can now focus on contributing more demos and leave
other works to `DemoCards.jl`.

Check the [Simplest Demopage](@ref simplest_demopage) to see what this page
looks like with the minimal configuration.

## What DemoCards.jl does

The pipeline of [`makedemos`](@ref DemoCards.makedemos) is:

1. analyze the structure of your demo folder and extracts supplementary configuration information
2. copy "`assets`" folder without processing
3. preprocess demo files and save it
4. process and save cover images

Since all files are generated to `docs/src`, the next step is to leave everything else
to `Documenter.jl` 😃

!!! tip

    By default, `makedemos` generates all the necessary files to `docs/src/democards`,
    so it's recommended to add it to the `.gitignore` of your project.

For advanced usage of `DemoCards.jl`, you need to understand the core [concepts](@ref concepts) of it.

## Some known issues

!!! warning

    It's better to save your demo files outside `docs/src`.

    If you store all your source demo files in `docs/src`, e.g., `docs/src/demos`,
    `DemoCards.jl` will make a copy of it in `docs/src/democards`. This means
    `Documenter.jl` will copy and process the same files _twice_.

!!! warning

    Currently, there's no guarantee that this function works for untypical
    documentation folder structure. By the name **typical**, it is:

    ```julia
    .
    ├── Project.toml
    ├── docs
    │   ├── demos # manage your demos outside docs/src
    │   ├── make.jl
    │   ├── Project.toml
    │   └── src
    ├── src
    └── test
    ```
