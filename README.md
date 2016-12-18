# FLEx-TeX

## What is FLEx-TeX?

FLEx-TeX is a utility written in Python that produces attractive and highly configurable LaTeX dictionaries from linguistic fieldwork databases created using SIL’s FLEx. It is intended for users who are familiar with FLEx and their own lexicon as well as how to perform basic markup in LaTeX.

## Requirements

If not using Windows, for which a standalone executable exists, Python 2.5 or later is required. (Python comes standard with Mac OS and most Linux distributions. If using Python, you will also need python-tk installed. Also note that Python 3 is still untested.)
Your language’s lexicon in FLEx 6 or later
LaTeX (XeTeX/XeLaTeX if using Unicode characters)

This version of FLEx-TeX does not support dictionary outputs from SIL’s Toolbox. The alpha version of this program was Toolbox-compatible, but it worked from a different output format that was much more difficult to configure; please contact me at the e-mail address below for a copy of that version if you need it.

## Use the FLEx Dictionary View to set up fields

From your Lexicon in FLEx 6/7, select Dictionary from the top-left pane. The presence and ordering of various fields can be set in Tools > Configure > Dictionary. In this stage you will not be making any decisions as to how these fields are formatted, but only which ones will appear and in what order; all this can be done from this configuration window. Note that it is recommended to use a stem-based rather than root-based dictionary.

You should also be sure at this point to exclude any entries that should not appear in the dictionary (bound morphemes, for example). This can be done by using filters in Lexicon Edit mode; any entries excluded by filters will also be excluded from the Dictionary view.

Also ensure that all words to appear have a headword defined! That is, the first field that your configured dictionary outputs should be present in every included entry. To easily force this to be the case, filter your headword (in FLEx’s Lexicon Edit) so that only non-blanks appear. (FLEx-TeX will treat the first field it encounters in each entry as the headword, so in cases with no headword present this may end up being something else―part of speech, for example.)

When finished, you should export a Configured Dictionary in XML format from File > Export.

## Configure alphabet

In the likely case that your language uses an alphabetization scheme different from English―and this includes cases of digraphs, accented characters, etc.―then you will want to set up an alphabetization file. This file should be saved in a two-column tab-separated format; spreadsheet editors will export in this format (probably with a .csv extension), although it is probably easier and more accurate to just use a text editor. Each line is considered a single letter in the dictionary, with a single tab separating the letter heading from all of the letters considered equivalent under that heading (which are in turn separated by spaces). Examples of letters that might be equivalent are lower/uppercase, accented and unaccented vowels, etc. (If this file is edited in a spreadsheet, the first column will show the letter headings and the second will have tokens of the letters themselves.) The included example alphabets illustrate this format.

When the script is run, you will be asked to locate this alphabet file. If you do not provide one, English alphabetization will be used.

## Run the script

You should be able to just double-click on FLEx-TeX.exe or FLEx-TeX.py, although it may be necessary to run the script from a Python interpreter if running the latter. The output of the script will be placed in a folder with the date and time in the format YYYY-MM-DD_HHMM.

By default, FLEx-TeX will sort all your entries alphabetically according to the scheme provided. You can disable this by unchecking the associated box.

You will be asked to locate four files. The first file is your XML output from FLEx. You will also be given the option to provide a custom alphabetization scheme, a configured custom commands file (optional; see below), and a LaTeX main file (optional to run the script but ultimately necessary; see below).

## About the output

The output of the script will contain at least two LaTeX files:

    all_field_types.tex Contains a custom LaTeX command for each type of dictionary field.
                This will have to be edited to your formatting preferences.
    entries.tex     Contains the entries themselves; this can probably stay as it is.

If you specified a main file at runtime, then this will also be copied to the output folder. The main file will include everything in your dictionary except the entries and the custom commands―so, all header and formatting information, title, etc. The file should also, crucially, contain two lines:

\input{all_field_types}     % In the preamble
\input{entries}         % Where you want the dictionary content to appear

An example of a main file is included with FLEx-TeX (“example main file.tex”). When finally compiling the dictionary in LaTeX, you will want to compile the main file, as the others will not compile on their own. If you are using any Unicode characters, be sure to compile under XeLaTeX (included in most LaTeX distributions). And if you are using Unicode and experience any issues at all with characters not showing up properly, check to be sure that all files are saved with UTF-8 encoding. (Notepad under Windows doesn't always do this properly, so if your characters aren't coming out correctly, try a different text editor.)

## Configure LaTeX commands

As indicated above, all_field_types.tex will have to be modified to suit your preferences in formatting the dictionary. Each field type will be given its own custom command. These commands can be edited to specify how each field will appear―for example, in boldface, in smaller font, with a following period or comma, etc.―using standard LaTeX syntax. In this example:

\newcommand{\LexSenseDefinition}[1]{\textbf{#1}}

a command is being defined that takes one argument and represents that argument in boldface; ‘#1’ stands for the argument. If this line is included in all_field_types.tex, then all definitions given in the dictionary will be bold.

The names of the fields can be rather bizarre and are determined by FLEx; if you’re not sure what the names refer to, you can check entries.tex or the XML file itself (open it up in a web browser or text editor) to see what content is associated with which field name. (Note that FLEx uses underscores for some XML tags; since underscores are not allowed in LaTeX commands, FLEx-TeX removes them in the LaTeX files, so the custom command names might not perfectly match the XML tag names. They may also contain information about the writing system used so that you can configure different outputs for multilingual dictionaries.)

As with the main file, you can also supply a prepared field types file (probably modified from an earlier FLEx-TeX output), which will then be copied to the output folder as all_field_types.tex; the newly generated generic file will be saved alongside it as all_field_types_blank.tex.
