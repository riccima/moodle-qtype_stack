# Displaying mathematics for STACK in moodle #

STACK generates LaTeX code on the fly and expects to use a moodle filter to convert this into something the user's browser will display.  There are a variety of mathematics filters for moodle.  Currently when installing STACK there is the option to choose which filter mechanism is used.   This enables STACK to fine tune the output.  Support is currently provided for

1. MathJax, <http://www.mathjax.org>, the default and is strongly preferred.
2. Moodle Tex filter.  This support is now considered legacy and has not been tested for some time.

One reason for adopting MathJax is that it aids [accessibility](../Students/Accessibility.md).

These instructions are adapted from http://moodle.org/mod/forum/discuss.php?d=193064

How to install MathJax depends on which version of Moodle you have.
1. For Moodle 2.7 or later, MathJax is distributed as a filter.  Please use this.
2. For earlier versions (e.g. 2.6 and before) you will need to link to MathJax yourself as below.


## Using the MathJax filter

These are notes on configuring the MathJax filter options for use with STACK.


1. Do not use "TeX filter compatibility" unless you want all equations to appear inline!
2. To add suppport for [actuarial notation](../Authoring/Actuarial.md) you need to add the option 'enclose.js' to the extensions.  I.e. within the "MathJax configuration" text we need the the following.   

    TeX: { extensions: ['enclose.js'] }


## Adding MathJax to additional HTML

If the MathJax filter is not available then you can include it in the head of every page by adding additional HTML to every page.

### Option 1: link to external MathJax setup. ###

1. Admin -> Appearance -> Additional HTML -> Within HEAD
2. Put the following script in it and save

`<script type="text/x-mathjax-config"> MathJax.Hub.Config({`<br>
`        MMLorHTML: { prefer: "HTML" },`<br>
`        tex2jax: {`<br>
`            displayMath: [['\\[', '\\]']],`<br>
`            inlineMath:  [['\\(', '\\)']],`<br>
`            processEscapes: true`<br>
`        },`<br>
`        TeX: { extensions: ['enclose.js'] }`<br>
`      });`<br>
`</script>`<br>
`<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_HTML"></script>`

Please note the following.

* These settings enable only the strict LaTeX maths environments, and does not support the use of dollars;
* The `processEscapes` flag enables you to include a dollar symbol in mathematics environments with `\$`.
* The line `TeX: { extensions: ['enclose.js'] }` enables support for [actuarial notation](../Authoring/Actuarial.md).

### Option 2: install MathJax locally. ###

A local installation may be preferable, e.g., if you want your STACK-equipped Moodle to work off-line, or if you want to try to speed up LaTeX rendering.

1. Download [MathJax](http://www.mathjax.org/)
2. Unpack the archive and rename folder to "mathjax".
3. Place the mathjax folder in .../moodle/lib and set appropriate ownership, e.g., `chown -R root.root mathjax`.
4. Carry out the procedure in Option 1, editing the script above to reflect your path to MathJax. The last line can resemble the following:<br>
`<script type="text/javascript" src="http://localhost/moodle/lib/mathjax/MathJax.js?config=TeX-AMS_HTML"></script>`

### What about dollar mathematics delimiters? ###  {#delimiters}

Please note that we strongly discourage the use of dollar symbols for denoting LaTeX mathematics environments such as `$...$` and `$$...$$` for inline and displayed mathematics respectively.  
The reasons are (1) they do not match, which makes parsing more difficult, and (2) many courses use [currency](../Authoring/CASText.md#currency) which needs this symbol and protecting it is always problematic.

If you have extensive existing materials using these delimiters then we have scripts which will automatically convert them for you.  All fields within existing questions can be converted with the automatic scripts available from

    Home > Site administration > Plugins > Question types > STACK

If you have ad-hoc text to convert then the CAS chat script *always* converts dollars into the forms `\( .... \)`and `\[ .... \]`.  You can test and edit the display of text in this window to see the conversion. 

If you really want to use dollars, MathJax can display them with the code

`        displayMath: [['$$', '$$'], ['\\[', '\\]']],`<br>
`        inlineMath:  [['$',  '$' ], ['\\(', '\\)']],`

Before deciding to support the use of dollars for mathematics on your site, please see the notes on the use of dollars for [currency](../Authoring/CASText.md#currency).

## Accessibility ##

The accessibility features supported by MathJax are given detailed [here](http://www.mathjax.org/resources/articles-and-presentations/accessible-pages-with-mathjax/).
