---
title: "LaTeX Tips"
date: 2022-11-18
---

## Using Verbatim in Beamer

To use `\verb`, `\verbatim`, or `\Verbatim` in Beamer, we need to pass
`[fragile]` option to the frame.

```latex
\begin{frame}[fragile]
    \frametitle{a fragile frame}
    \begin{verbatim}
some verbatim text here
    \end{verbatim}
\end{frame}
```

Also be mindful of the whitespace at the beginning of your vebatim
portion. Although LaTeX usually takes care of whitespaces properly,
since it's a verbatim portion, LaTeX will render exactly as you put it.


## Duplicate a Slide in Beamer

```latex
\documentclass{beamer}

\title[Short title]{The title}
\author[Authors]{Author 1 \and Author 2}
\date{\today}
\begin{document}
\begin{frame}[label=firstframe]
\titlepage
\end{frame}

% ...

\againframe{firstframe}
\end{document}
```

## Latex on this website (Note to self)

Wrap with `$$` signs or in `\[...\]` signs for latex blocks: 

$$E=\left(\frac{c^{2}}{32\pi G}\right)\omega^{2}f^{2}$$

And `\(...\)` for inline latex, like this:
\(\hbar\omega \times \frac{\omega^3}{c^3}\)
