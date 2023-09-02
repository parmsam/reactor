# Reactor
_Reactive notebooks for R_

<!-- badges: start -->
  [![Travis build status](https://travis-ci.org/herbps10/Reactor.svg?branch=master)](https://travis-ci.org/herbps10/Reactor)
<!-- badges: end -->

**This is experimental software**. There are bugs, and the API is liable to change without maintaining backwards compatibility.

## What is it?
Reactor notebooks are collections of cells containing R code. When you update a cell, all of the cells that reference it are automatically updated, like how a spreadsheet works. Reactor notebooks integrate R code, plots, HTML, and markdown into one document.

Reactor notebooks are useful for prototyping code and exploring subjects through interactive visualizations.

<a href='https://hsusmann.shinyapps.io/gaussian_processes/'><img src='https://i.imgur.com/2Los4zE.png' width='75%' /></a>

Reactor notebooks can be shared online as [Shiny](https://shiny.rstudio.com) applications. You can play with an [example notebook](https://hsusmann.shinyapps.io/gaussian_processes/) which is available online through Shiny.

## Demo video
The [demo video](https://www.youtube.com/watch?v=2GViKLqthZo&feature=youtu.be) on YouTube shows how to use Reactor to build a simple interactive notebook:

<a href="http://www.youtube.com/watch?feature=player_embedded&v=2GViKLqthZo
" target="_blank"><img src="https://img.youtube.com/vi/2GViKLqthZo/mqdefault.jpg" 
alt="Reactor demo: interactive notebooks for R" width="320" height="180" border="10" /></a>

## Getting started
Install and load `reactor`:
```r
devtools::install_github("herbps10/reactor")

library(reactor)
```

Create a new notebook and launch the Reactor server:
```r
# Create new Reactor notebook
notebook <- ReactorNotebook$new()

# Launch server at http://localhost:5000
server <- start_reactor(notebook)
```

Save progress and stop the server:
```r
# Save progress
notebook$save("./notebook.rds")

# Stop server
stop_reactor(server)
```

Load the notebook later to start where you left off:
```r
# Load notebook
notebook <- ReactorNotebook$load("./notebook.rds")
```

Reactor includes an example notebook:
```r
# Load Gaussian Process example notebook
notebook <- reactor_example("gaussian_processes.Rmd")

server <- start_reactor(notebook)
```

You can also see and interact with the example notebook running as a [Shiny application](https://hsusmann.shinyapps.io/gaussian_processes/).

## Features

### Reactive execution
If a cell is used to define a variable, Reactor keeps track of all the other cells that depend on it. If you update the variable, all the dependent cells are rerun.

<img src="https://thumbs.gfycat.com/HarmoniousGroundedEquestrian-size_restricted.gif" width="100%" alt="Example of reactive execution" />

### Interactivity
Interactive inputs can be used to set the value of an R variable.

<img src="https://thumbs.gfycat.com/SickCircularLeonberger-size_restricted.gif" width="100%" alt="Example of interactive inputs" />

### Plotting
Reactor supports base plots and ggplot2.

<img src="https://thumbs.gfycat.com/ParchedMedicalAardvark-size_restricted.gif" width="100%" alt="Example of interactive HTML widgets" />

### Widgets
Any R variable with the class "htmlwidget" will be rendered as HTML. 

<img src="https://thumbs.gfycat.com/GrizzledSlowLacewing-size_restricted.gif" width="100%" alt="Example of interactive HTML widgets" />

### Saves to Rmd
Reactor notebooks are saved as [R markdown](https://rmarkdown.rstudio.com/articles_intro.html) files, which you can open and edit like any other Rmd file. You can see examples of notebook files in the [`inst/examples`](https://github.com/herbps10/reactor/tree/master/inst/examples) folder.

### Run as Shiny
Reactor notebooks can be run as Shiny applications, making it easy to deploy notebooks online for sharing with others. See `vignette("shiny_deployment")` for an example of deploying a notebook to [shinyapps.io](https://www.shinyapps.io).

## And more

- View documentation in a side panel by calling it up from a cell (e.g. `?lm`) or the shortcut Ctrl-Shift-?.
- Export notebooks to R scripts, with the cells rearranged to run from top to bottom.

## Comparison to existing tools

Reactor is inspired by [Observable](http://observablehq.com), which provides a similar notebook interface for Javascript. I've been very happy with the Observable workflow, and wanted to be able to use a similar interface with R so I could access more heavy duty statistical tools. In R, the package [Shiny](https://shiny.rstudio.com) is similar, in that it supports reactive execution for R, but it doesn't currently provide the ability to author code (a new package, [shinymeta](https://rstudio.github.io/shinymeta), does allow for exporting R code that Shiny generated reactively.) [Jupyter](https://jupyter.com) notebooks are a very popular notebook interface for various backend languages, but it generally does not enforce any execution order for its cells. The [dfkernel](https://github.com/dataflownb/dfkernel/) project extends Jupyter notebooks for Python to enable a reactive execution flow. 

|                                      | Language   | Authoring | Reactive |
|--------------------------------------|------------|-----------|----------|
| Reactor                              | R          | ✔         | ✔        |
| [Pluto.jl](https://github.com/fonsp/Pluto.jl)    | Julia      | ✔         | ✔        |
| [Shiny](https://shiny.rstudio.com/)   | R          |           | ✔        |
| [Observable](https://observablehq.com)| Javascript | ✔         | ✔        |
| [Jupyter](https://jupyter.com)               | Various    | ✔         | For Python with [dfkernel](https://github.com/dataflownb/dfkernel/)        |
| Spreadsheets                         | Various    | ✔          | ✔ |

## Todo list

- [x] export to R script 
- [ ] export to HTML
- [x] run in shiny
- renderers
  - [x] markdown
  - [x] LaTeX
  - [x] HTML
  - [x] matrix
  - [x] function
  - [ ] data.frame/tibble
  - [ ] vectors
- HTML inputs:
  - [x] range/slider
  - [x] number
  - [x] checkbox
  - [ ] radiobox
  - [x] text
  - [ ] textarea
  
## Troubleshooting

You will need to ensure that some other process is not sitting on port 5000. On Macbooks, port 5000 may be taken up by Airplay functionality and require turning off Airplay Receiver in order to open the port.