# neotest-scala

[Neotest](https://github.com/rcarriga/neotest) adapter for scala.

Supports [utest](https://github.com/com-lihaoyi/utest), [munit](https://scalameta.org/munit/docs/getting-started.html), [ScalaTest](https://www.scalatest.org/) and [Specs2](https://etorreborre.github.io/specs2) test frameworks, by either running it with [bloop](https://scalacenter.github.io/bloop/) or [sbt](https://www.scala-sbt.org).

Note that for ScalaTest the only supported styles are FunSuite and FreeSpec for now, and for Specs2 only mutable.Specification style is supported.

It also supports debugging tests with [nvim-dap](https://github.com/rcarriga/nvim-dap) (requires [nvim-metals](https://github.com/scalameta/nvim-metals)). You can debug individual test cases as well, but note that utest framework doesn't support this because it doesn't implement `sbt.testing.TestSelector`. To run tests with debugger pass `strategy = "dap"` when running neotest:

```lua
require('neotest').run.run({strategy = 'dap'})
```

Requires [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter) and the parser for scala.

## Installation

Using [packer.nvim](https://github.com/wbthomason/packer.nvim):

```lua
use({
  "nvim-neotest/neotest",
  requires = {
    ...,
    "stevanmilic/neotest-scala",
  }
  config = function()
    require("neotest").setup({
      ...,
      adapters = {
        require("neotest-scala"),
      }
    })
  end
})
```

## Configuration

You can set optional arguments to the setup function:

```lua
require("neotest").setup({
  adapters = {
    require("neotest-scala")({
        -- Command line arguments for runner
        -- Can also be a function to return dynamic values
        args = {"--no-color"},
        -- Runner to use. Will use bloop by default.
        -- Can be a function to return dynamic value.
        -- For backwards compatibility, it also tries to read the vim-test scala config.
        -- Possibly values bloop|sbt.
        runner = "bloop",
        -- Test framework to use. Will use utest by default.
        -- Can be a function to return dynamic value.
        -- Possibly values utest|munit|scalatest.
        framework = "utest",
    })
  }
})
```

## Roadmap

To be implemented:

- Displaying errors in diagnostics
- Detect what test library is used
- Don't block neovim when trying to figure out the project name for building test commands
- Pick runner based on what is configured in metals? (Metals runs bloop / bsp)
