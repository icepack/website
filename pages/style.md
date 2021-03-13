<!--
.. title: Style
.. slug: style
.. date: 2020-09-18 08:25:57 UTC-07:00
.. tags:
.. category: 
.. link: 
.. description: 
.. type: text
-->

Icepack is a community project and we gratefully accept contributions from anyone.
At the same time, we also want to keep the code as readable and maintainable as possible and that involves a bit of discipline.
The Python development team has published a very general set of guidelines for Python coding style called [PEP8](https://pep8.org/) and we try to conform to this as best we can.
There are specific considerations around coding style for scientific software that we’ll try to address too.

### Variable naming

An old quote that's become folklore by now: "There are only two hard things in computer science, cache invalidation and naming things."

Throughout icepack, we employ two different guiding principles for how to name identifiers.
Some code is meant to be read by everyone – glaciologists, curious oceanographers, or us, the maintainers of the project.
For example, everyone will need to call the diagnostic and prognostic solve routines for some flow solver.
In these parts of the code base it's critical to use English names that clearly and succinctly indicate the role of the object, like `velocity`, `thickness`, `viscosity`, `solve`, and so forth.

Other parts of the code are only meant to be read by someone who is familiar with the physics and mathematics.
**Mathematical code should look as much like the mathematics as possible.**
The internal details of what makes up the viscous part of the action functional for a particular ice flow model is a good example.
If you are reading that code, you should know that it will involve quantities like the strain rate tensor, and you should be familiar with the fact that this field is usually denoted with a Greek letter epsilon in most books on continuum mechanics.
In these parts of the code, we freely use single-character identifiers like `u` for velocity, as long as they match what appears in textbooks.
We will even sometimes use Greek letters like `ε` or `φ` where appropriate.
(Most text editors include shortcuts for inserting these symbols.
For example, in vim, you can insert a Greek letter `φ` by entering insert mode, typing `ctrl + k`, and then typing `f`.
In a Jupyter notebook, you can start typing `\phi` and then hit the tab key.)
These are never to be used in the public user interface, but they can make the code more mnemonic and familiar once you know the mathematics.
The kind of code where you would write like this will be written once, debugged twice, and read hundreds or thousands of times.
Make it look like the math we all know and love, and it'll be that much easier to pair up with your preexisting conceptions of what that code is for.

### Style

We use the automatic formatter [black](https://black.readthedocs.io/en/stable/) for all code in icepack.
Black will do things like normalize all quotes, wrap long lines, and generally fix annoying stylistic issues that aren't worth the time you spend on them.
The style that it enforces isn't perfect but it helps reduce entropy growth.
To run it, first `pip3 install black` and then do:

```shell
black ./
```

This will edit any Python source files in the current directory in-place.
Formatting is part of the test suite -- pull requests will fail if the code hasn't been formatted.

We've also developed a few coding conventions outside of what black enforces:

* In internal code, use [operator.itemgetter](https://docs.python.org/3/library/operator.html#operator.itemgetter) from the standard library to fetch several values from a dictionary by key.
This pattern appears everywhere internally because all physical fields are passed by keyword to the various solvers.
* In tutorial code, fetch every value separately only its own line.
* Use [f-strings](https://docs.python.org/3/tutorial/inputoutput.html) instead of `.format`.
* When a line of code grows too long, find sub-expressions that can be given obvious and informative names and hoist them into local variables, even if they'll only be used once.

These aren't universal rules that apply to all Python code, they're just cases where there's more than one way to do something and we had to pick a way in order to keep the code consistent.
