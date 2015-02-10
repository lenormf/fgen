# fgen
`fgen` is a minimalistic file generator, using python's template compilation with the curly braces syntax.

## Usage

```
usage: fgen.py [-h] [-d] [-v] [-f FILENAME] [-t TEMPLATES] [-s SEPARATOR] [-l]
               template [variable:value [variable:value ...]]

A minimalistic file generator that uses python templates with the curly brace
syntax

positional arguments:
  template              Name of the template to use (no extension needed)
  variable:value        Variables to pass to the template context, and their
                        value

optional arguments:
  -h, --help            show this help message and exit
  -d, --debug           Display debug information
  -v, --verbose         Display more information
  -f FILENAME, --filename FILENAME
                        Name of the file that will be generated (default: name
                        of the template in the current working directory)
  -t TEMPLATES, --templates TEMPLATES
                        Path to the directory that contains all the templates
  -s SEPARATOR, --separator SEPARATOR
                        Separator to trim between the key and the value of the
                        context parameters
  -l, --list            List all the templates available from the templates
                        directory
```

## Getting started

Here's the few things to do before getting started:
* create a directory named `templates` in your home directory
* create a file in that same directory, named `test.txt` and containing the string `Hello World ${name}!`
* `cd` to another directory, so as to not mistakenly erasing a template (read why in the next paragraph)

Once that's done, execute the following command (assuming that fgen is accessible through your PATH):
`fgen test name:"$USER"`

You should now be able to open the newly created `test.txt` file, and see that it contains the `Hello World your_user_name!`.

## How it works

`fgen` requires only one argument to generate a file after a template: the name of the template itself. The name of a template is the name of the file in which the template was stored, without the extension (although you can pass the full filename to `fgen` to avoid conflicts).

Once the template was found in the templates directory and loaded, it is compiled with a context, made of all the variables supplied on the command line interface. Those variables are entirely optional, and have to be supplied to `fgen` in the following format: `name:value`.

Templates are all located in your home directory, in the directory called `templates`, and are basically text files containing code or data that you tend to use often. You can list all the available templates with the `-l` option, or `--list`.

The name of the file that will be generated is the fullname (name + extension) of the template used, unless the `-f` flag is used.

Finally, all the default settings are tweakable with command line flags, and/or with the `Defaults` class in `fgen.py`.

## Corner cases

Since `fgen` is a simple&stupid© script, there are a few corner cases that I was not really bothered fixing:
* files created after a template will overwrite files that already exist in the current working directory
* if a template variable is not provided with a replacement value (→ no key:value pair is passed to the command line interface), it will be inserted as-is in the generated file
* it is not possible to pass filters to the variables in the templates (no automatic uppercase, lowercase etc)
* if two templates have the same name (without extension), the one that will be used will be picked randomly (as per [`os.listdir`](https://docs.python.org/2/library/os.html#os.listdir)'s documentation)
