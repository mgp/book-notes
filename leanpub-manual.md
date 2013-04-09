## Leanpub Manual

* Markdown supports almost all the Kramdown extensions, with the exception of HTML blocks and `<<` becoming the left guillemet.
* Attributes should be alone on a line, with blank lines above and below.
* Technical books are on 8.5"x11" paper with inch margins, leaving 6.5"x9" to work with.
* With 300PPI images, an image can be 1950x2700 pixels, while the cover page should be exactly 2550x3300 pixels.
* You can float and align an image on your page by using the `width` and `float` attributes.
* Cover images can be in either JPEG or PNG format and must be named `title_page.jpg` or `title_page.png`.
* If all your books are in stealth mode, then your profile is also invisible to the public.
* When specifying whether libraries can purchase your book, consider that libraries can lend out an unlimited number of copies of your book without DRM.
* You can add Google Analytics to your landing page.
* Leanpub ignores all `.git` directories, and so you can use a Git repository as your `manuscript` directory.
* To add a motto or epigraph to the beginning of a chapter, simply center the text.

### Markdown

* To start a new part in your book, start a line with `-#` followed by the part title.
* Create `frontmatter.txt`, `mainmatter.txt`, and `backmatter.txt` files that just contain `{frontmatter}`, `{mainmatter}`, and `{backmatter}`.
* Surround text in carets (`^`) to make it superscript.
* Adding two spaces at the end of a paragraph creates separation. This is useful when following a paragraph with another kidn of text block.
* You can center text by preceding it with `C> `.
* Once you start a numbered list, it doesn't matter what number you put at the beginning of the line.
* To put a code block in a list, indent it by 8 spaces, and place a blank line before and after the code block.
* For a definition list, put the thing you want to define on a line by itself, and on the next line precede the defition with a colon (`:`).
* Text in a blockquote is preceded with `> `.
* Text in an aside, or sidebar, is preceded with `A> `.
* In the same style as asides, use `W> ` for warnings, `T> ` for tips, `E> ` for errors, `I> ` for information, `Q> ` for questions, `D> ` for discussions, and `E> ` for exercises.
* Create a code block by placing matching numbers of tildes before and after the block.
* You can put code in its own folder, and refer to it with either `<<(code/filename.ext)` or `<<[title](code/filename.ext)`.
* To create a link without alternative text, surround the URL in angle brackets (`<` and `>`).
* There must be a blank line before and after a footnote definition, and the caret symbol is required.
* Identifiers that you can crosslink to are preceded with `#` and enclosed in curly braces (`{` and `}`).
* To force a page break, add `{pagebreak}` on a line by itself.
* When defining a table, use vertical bars (`|`) to separate columns; also place bars before the first and after the last column.
* To add more spacing between rows in a table, put a dash lined between them.
* To exclude lines, you can precede the text with two `%` characters.
* To use inline or block LaTeX math, surround the LaTeX with delimiters `{$$}` and `{/$$}`.
* Curly quotes are automatically generated from striaght quotes in your Markdown.
* A hack to add more space between paragraphs is to add a blank table, defined as `| |`.

