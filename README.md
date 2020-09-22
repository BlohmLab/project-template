# project-template

# Goals for this repository
This repository exists for a few reasons. 
In general, it is part of our lab's ongoing effort to publish clean, consistent, and reproducible code in the pursuit of [Open Science](https://www.cos.io/).
More specifically, this repository serves three purposes:

1. It provides a starting point for lab members who may be new to organizing a Python data science project.
2. It provides examples of common data science tools and conventions that may be useful to lab members who are already comfortable with Python data science.
3. It provides a Bash script for generating a standard repository skeleton to help start new Python projects quickly.

The repository structure and workflow provided is **not intended to be presecriptive.**
There are obvious benefits to having a common repository structure, but [a foolish consistency is the hobgoblin of little minds](https://www.goodreads.com/quotes/353571-a-foolish-consistency-is-the-hobgoblin-of-little-minds-adored).
Every person has their preferred workflow, and every project has different needs.
Feel free to ignore/update/rename any component of this template as needed.

# Directory structure 
```
.    
├── data    
│   ├── 01_raw    
│   ├── 02_interim    
│   └── 03_processed    
├── models    
├── notebooks     
│   └── jupytext     
├── reports     
│   └── figures     
├── my_project    
│   ├── __init__.py     
│   ├── data    
│   ├── models    
│   ├── utils    
│   └── visualization       
├── README.md     
├── makefile    
├── requirements.txt     
└── setup.py     
```

## Data

## Notebooks and Jupytext

## Reports and Figures

## Source (my_project)

## Models

## Setup.py and requirements

## Makefile 
The top-level directory of this project contains a file called `makefile`.
This is not a python or shell script, but a unique text file that contains instructions (or "recipes") to be carried out by a GNU utility called `make`.
Makefiles are commonly used when writing C programs to help with code compilation.
This utility is installed by default on Linux systems, but can be installed with one command on macOS via [homebrew](https://formulae.brew.sh/formula/make) (`brew install make`) or Windows via [chocolatey](https://chocolatey.org/packages/make) (`choco install make`).

Python is an [interpreted](https://en.wikipedia.org/wiki/Interpreted_language) language that doesn't need to be compiled before running, so why would we use a C compilation utility for a Python data science project?
[This blog post](https://krzysztofzuraw.com/blog/2016/makefiles-in-python-projects) provides some motivation, but the short answer is "because it can make your life easier."

The quickest way to understand how a Makefile works is to `cd` into the top directory of the repository (where `makefile` lives) and type:

```
$ make
>> This is the first recipe in the file.
```

This should print the above text to your terminal. Let's try:

```
$ make hello
>> Hello!
```

Open `makefile` with a text editor and take a look. 
Ignore the first line (`.PHONY = ...`) for now, and instead check out the four "recipes" included in the makefile.
Recipes look (and operate) in an analagous way to functions in other languages.
We can call one of the recipes from a terminal by typing `make <recipe name>`.
In our case, the recipes simply contain Bash commands.
When we type `make hello`, the shell will execute `@echo "Hello!"` and we will see the output as if we had typed `echo "Hello!"` into our terminal.
The "@" prevents make from also printing the echo command back to us.

Let's try another recipe:

```
$ make all
>> This is the first recipe in the file.
```

This is the same output that we got when we typed `make`; make executes the first recipe in the file by default if you don't provide a recipe name.
In practice, it can be useful to have your first recipe simply list the various recipe options, so that you (or someone else) can quickly find a command without opening the makefile.

Recipes can be used for a lot more than just printing text.
In general, you can think of a Makefile as a place to store terminal commands or series of terminal commands that you want to execute fairly often, but can't be bothered to type out manually each time.
For example, this recipe can be used to sync Jupyter notebooks with Jupytext (see the [Jupytext](#notebooks-and-jupytext) section for more info):

```
notebooks: 
	@jupytext --set-formats ipynb,jupytext//py --sync notebooks/*.ipynb
	@jupytext --sync notebooks/jupytext/*.py
```

Instead of remembering and typing out these long commands every time you add a new notebook, you can plunk them into your Makefile and run them with `make notebooks`.

You can also use your Makefile to execute Python scripts.
For example, if you have a series of data processing scripts that must always be called in the same order, you could have a recipe like this:

```
data:
	@python data_script_1.py
	@echo "Step 1 complete."
	@python data_script_2.py
	@echo "Step 2 complete."
	@python data_script_3.py
	@echo "Step 3 complete."
```

The above examples should be enough to get you started using make to automate things.
A few closing notes:

- The `.PHONY = ...` line at the top of the Makefile simply tells make that the recipe doesn't reference a specific file (if we were using make for compilation, recipes would typically be asscociated with specific code). When you add a new recipe, add its name to this list so that make knows these are just general-purpose commands.
- Make interprets spaces and tabs differently! When indenting inside a recipe, make sure you use tab.