# Basic Syntax
Comments are made using `%`, and can also be used to 'escape' newline characters.

There are 4 kinds of dashes.
- `-` Normal dash (hyphen) used for compound words/word splitting.
-  `--` En-dash used for writing a range of numbers.
- `---` Em-dash to mark an interruption in speech/abrupt change in thought.
- $-$ Subtraction symbol.

 To insert a forward slash (`read/write`) use the `\slash` command since it permits hyphenation.

By default LaTeX inserts slightly more spacing at the end of a sentence for readability. If a period follows an uppercase letter, the extra space might not be inserted. 
- Use tilde characters (`~`) to create non-breaking spaces, preventing line breaks. Good when referencing figures or anything that shouldn't be split (`Fig.~5`).
- Use `\@` in front of a period to terminate the sentence (add the extra spacing at the end of the sentence). Example: `Example of usage\@.`

Use `*` in sectioning commands to omit heading numbers and prevent the section from appearing in the table of contents.
```latex
\section*{Introduction}
```

# Commands
Commands begin with a backslash `\` symbol. Commands can accept arguments/parameters, which are passed after the command name. For clarity, use 'groups' (enclose arguments in `{}`) to pass arguments
```latex
% The command 'foo' has the argument 'bar', 'baz', and 'q', 'u', 'x'
\foo{bar}{baz}qux
```
Commands which do not take any parameters still ignore the space that comes after them. To maintain the space, pass an empty group to the command.
```latex
A \LaTeX sentence % A LaTeXsentence
A \LaTeX{} sentence % A LaTeX sentence
```
Optional parameters are passed to commands using `[]` brackets, and usually come right after the command name.
```latex
\command[]{}
```

Custom commands can be declared:
```latex
\NewDocumentCommand{\name}{}{definition}

% Example command that prints a complicated name. The % are used to escape newlines
\NewDocumentCommand{\lukas}{}{% 
Łukasiewicz% 
}
```
An error occurs if the command name is already defined. Existing commands can be redefined as well:
```latex
\RenewDocumentCommand{\name}{}{definition}
```


| Command                    | Description                                                                                            |
| -------------------------- | ------------------------------------------------------------------------------------------------------ |
| `\emph{text}`              | Emphasize the text.                                                                                    |
| `\ldots`                   | Inserts proper ellipsis ($\ldots$).                                                                    |
| `\newline` or `\\[length]` | Starts a newline.                                                                                      |
| `\\*[length]`              | Starts a newline but prohibits a page break after it.                                                  |
| `\newpage`                 | Forces a newpage.                                                                                      |
| `\linebreak[n]`            | "Suggest" a linebreak with priority `n` (`0 < n < 4`).                                                 |
| `\nolinebreak[n]`          | Same idea.                                                                                             |
| `\pagebreak[n]`            | Same idea.                                                                                             |
| `\nopagebreak[n]`          | Same idea.                                                                                             |
| `\hypenation{word list}`   | Inform LaTeX about how a word should properly be hyphenated.                                           |
| `\-`                       | Insert a 'discretionary' hyphen in a word, which is only used if needed.                               |
| `\mbox{text}`              | Keep `text` together, preventing hyphenation or splitting the text onto different lines.               |
| `\frenchspacing`           | Disable the extra spacing at the end of sentences.                                                     |
| `\enquote{text}`           | Encloses the text in proper quotation marks. Can be used recursively.                                  |
| `\textquote[source]{text}` | When quoting a source in a more formal setting.                                                        |
| `\verb+text+`              | Write the text verbatim without treating anything enclosed as special. The delimiters are usually \| but can be anything else. |
|                            |                                                                                                        |











# Environments
Environments start with `\begin` and end with `\end`. 

| Environment   | Description                                                       |
| ------------- | ----------------------------------------------------------------- |
| `document`    | The main document environment that contains the body of the work. |
| `em`          | Emphasis environment that emphasizes all text within.             |
| `displayquote` | For longer block quotations.                                                                  |
|               |                                                                   |


# Packages
Packages provide more commands and functionality for use. They can be used by including this line in the 'preamble' (before `\begin{document}`):
```latex
\usepackage[options]{package}
```

| Package     | Description                                                                          |
| ----------- | ------------------------------------------------------------------------------------ |
| amsmath     | Standard math typesetting package.                                                   |
| polyglossia | LaTeX for multiple languages.                                                        |
| booktabs    | Better formatting for tables.                                                        |
| biblatex    | Creates bibliography easily.                                                         |
| makeidx     |                                                                                      |
| fancyhdr    | Customize headers and footers.                                                       |
| beamer      | Provides a document class that can be used to make presentations and typeset slides. |
| ragged2e    | Define `FlushLeft, FlushRight, Center` environments.                                                                                     |


