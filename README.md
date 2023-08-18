<div style="text-align: center">

<picture>
<source media="(prefers-color-scheme: dark)" srcset="./ganesha-logo-dark.svg" width=600>
<img src="./ganesha-logo-light.svg" width=600>
</picture>

[Presentation](#introduction) • [Documentation](#features) • [Contribute](#contributing)

![Release](https://img.shields.io/gitlab/v/release/Mwpal/teaching-template)
![Last commit](https://img.shields.io/github/last-commit/badges/shields/master)

![](./ganesha-doc.png)

</div>

---

# :om: Introduction
<a href="http://ultravioletbat.deviantart.com/art/Yay-Evil-111710573">
  <img src="./ganesha-lord.svg" align="right" width=200/>
</a>

> Ganesha, deity in the Hindu pantheon, is said to be the son of *Parvati* and *Shiva*. He is the parton of arts & sciences, and the
> deva of intellect and wisdom. Amongs its many attributes, Ganesha is readily identified by his elephant head and his four arms.
> Lord Ganesha has two wives, one named Siddhi (Success) and the other named Riddhi (Prosperity). **This is the LaTeX stylesheet he uses.**

Ganesha is a stylesheet for LaTeX, tailored for writing Lecture Notes, Labs and Tutorials.
It especially focus on those aspects:

- **Minimalism.** The template style is pretty simple, to use focus on the content and let space for handwritten annotation along the way.
- **Safety.** The template provides mecanisms to prevent awkward moments such that remaining TODO's or displayed proofs within the printed version destinated to students.
- **Simplicity.** The template relies on multiple external packages whose configuration is hidden. There is absolutely no need to master them for using the template.

[!WARNING]  
At the moment, the stylesheet is in conflict with the `Enotez` package, which is used for the in-line exercises features. Other packages achieve similar features if needed, such as `endnotes`.

# 📝 Start-up guide 

A more complete documentation can be infered from the .org file, where the source code of the stylesheet appears. We focus here on commands/features introduced by the stylesheet.

## Style
### Colors

The stylesheet relies on the gruvbox color theme, and colors are defined using a `g<color>` nomenclature. The colors used within the theme are `\fstAccent`, the first accent color (`gBlue` by default); and `\sndAccent`, the second accent acolor (`gPurple` by default). The later could be overwritten using `\renewcommand`.

```latex
\renewcommand{\fstAccent}{...} % e.g. first page header, section, highlighted text
\renewcommand{\sndAccent}{...} % e.g. algorithm, in-line exercise recap
```

You can switch those accent color using the `\switchAccents` command. Typically, this could be used before an appendix.

### Headers

The stylesheet define two headers. The first one, called "Pretty header" is the fancy one: a large `\fstAccent` rectangle at the begging of the page. It aims to be used at the beginning of the document (but could also serve as chapter header). If you want to use this header, you **must** define the informations you want within the header; otherwise, you will not be able to produce non-draft versions.

```latex
\renewcommand{\phForeword}{...}
\renewcommand{\phTopic}{...}
\renewcommand{\phNEinfo}{...}
\renewcommand{\phSEinfo}{...}
```

The second header is the default one, made of two informations you **must** define; otherwise, you will not be able to produce non-draft versions.

``` latex
\renewcommand{\headerleft}{...}
\renewcommand{\headerright}{...}
```

## Basic environments

The stylesheet configures a collection of basic environments: 

- `quote` is for quoting, the (mandatory) argument is the source of the quote.
- `definition`, `theorem`, `lemma`, `corollary`, `proposition` are rather transparent. The optional argument is for naming things (eg. "Pumping lemma" in theorical computer science).
- `proof` is for proofs. The optional argument could be used to highlight that a proof is "sketchy" or "incomplete".
- `algorithm` is a pretty environment for algorithms. The (mandatory) argument is the name/purpose of the algorithm. The syntax is the one provided by the `algpseudocodex` package and is rather intuitive. Two special keywords, `\Require` and `\Ensure`, allow respectively to specify the input and output of the algorithm, in a rather formal way.
- `sourcecode` is for displaying sourcecode, within a 80 characters long window. The (mandatory) argument is for the langage considered for syntax highlight. It relies on the `minted` package, so ensure `minted` and `Pygments` are installed (see [`minted` documentation](https://tug.ctan.org/macros/latex/contrib/minted/minted.pdf) for details).

An exemple of use is

``` latex
\begin{quote}[Britain's wartime propaganda department] 
Keep calm and carry on.
\end{quote}
```

Note that no remark, note, etc. environment exist. This is a design choice to enforce non-formal information to be genuinly written with main body.

## New environments

### Standard exercises

Writing standard exercises is done through the `questions`, `subquestions` and `question` environments. The way these environments interact with each other can be compared to the `columns`/`column` environments in Beamer.

An exercise is a set of questions, then (even if this is a one-question exercise) it is enclosed within a `questions` environment. Within the `questions` environment, you can use the `question` environment to write a question. A complex question can include subquestions, and this is handled by the `subquestions` environments. Note that a subsubquestion is nothing but a subquestion attached to a subquestion, hence this is also handled by the `subquestions` environment.

The question counter is global: the numeration continues through the different `questions` environments. You can use the `\newexercise` command to reset the counter.

Intertext, that is text between questions, may generate extraspace. The command `\noparskip` can be used (several times) manually to remove this space.

Here is an example of an exercise.

``` latex
\begin{questions}
  \begin{question}
    This is the first question
  \end{question}
  \begin{question}
    This is the second question. This is a complex question that will be
    decomposed in subquestions.
    \begin{subquestions}
      \begin{question}
        First subquestion.
      \end{question}
      This is intermediary text, just here for fun.
      Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque
      faucibus nisi justo, at finibus velit elementum non.  Vivamus blandit diam
      augue, ac suscipit velit iaculis sed.
      \begin{question}
        This is the second subquestion. This is a complex subquestion that will
        be decomposed in other subquestions.
        \begin{subquestions}
          \begin{hardquestion}
            This is the first subsubquestion. We stamp it as hard.
          \end{hardquestion}
          \begin{question}
            This is the second subsubquestion.
          \end{question}
        \end{subquestions}
      \end{question}
    \end{subquestions}
  \end{question}
\end{questions}
```

Finally, we stress that according to the *minimalist* philosophy of this template, using `subquestions` is in fact depraciated: please prefer using a one-number numeration and use main body to genuinly express the relation between questions.

### Answers

The `answer` environment behaves as the `proof` environment, but with a slightly different style.

### Inline exercises

We think good lecture notes include exercises along the way (eg. defering some 101-proofs to the reader). This template include a simple way of doing that.

The `\exo` macro include a small numbered "exo" box within the main body. Before the next `\exo` is dropped, you must give some details on the exercise via the `\exodetails` commands: typically, rephrase the exercise properly. Basically, one can think of `\exo` as a `\footnotemark`-like command and `\exodetails` as a `\footnotetex`-like one.

Finally, at the end of the document, use the `\printexercises` command. This will write down all the exercises you let along the way, specifically the `\exodetails` you gave.

### Hints

As its name suggests, the `hint` environment is the proper way to give hints on exercises. You can use them after a `question` environment or within the `\exodetails` command.

## Document version

The compilation will eventually be made via a `make` command. For the moment, you can add the following commands **before** including the `ganesha` stylesheet in order to change the style/purpose of the document.

- By adding `\let\printableversion=T%` to the document, you stamp it as a document to be printed:
  + The wide margin will alternate between right-hand and left-hand sides.
  + The hints will be displayed.
- By adding `\let\studentversion=T%` to the document, you stamp it as a document to be handed to students:
  + The answers will be hidden.
- By adding `\let\nondraftversion=T%` to the document, you stamp it as ready for share:
  + The "draft" watermark is removed
  + The compilation won't work if some `\nondrafterror{<Details on the error>}` are remaining. This command is present in default header name: you will not be able to ready-for-share compile if some default value of the stylesheet are visible. We encourage you to use this command for your TODO: for example, you could define
```latex
\newcommand\TODO{
  \textbf{\textcolor{red}{TODO}}%
  \nondrafterror{TODO cannot appear in non-draft version.}%
}
```

# :alembic: Contributing

If you find bugs or think of new features, please raise an issue! :blush:
