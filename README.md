[![DOI](https://zenodo.org/badge/142310332.svg)](https://zenodo.org/badge/latestdoi/142310332)

# Generating OpenMath Content Dictionaries from Wikidata

OpenMath content dictionaries are collections of mathematical symbols.
Traditionally, content dictionaries are handcrafted by experts.
The OpenMath specification requires a name and a textual description in English for each symbol in a dictionary.
In our recently published MathML benchmark (MathMLBen), we represent mathematical formulae in Content MathML referring to Wikidata as the knowledge base for the grounding of the semantics.
Based on this benchmark, we present an OpenMath content dictionary, which we generated automatically from Wikidata.
Our Wikidata content dictionary consists of 330 entries.
We used the 280 entries of the benchmark MathMLBen, as well as 50 entries that correspond to already existing items in the official OpenMath content dictionary entries.
To create these items, we proposed the Wikidata property P5610.
With this property, everyone can link OpenMath symbols and Wikidata items.
By linking Wikidata and OpenMath data, the multilingual community maintained textual descriptions, references to Wikipedia articles, external links to other knowledge bases (such as the Wolfram Functions Site) are connected to the
 expert crafted OpenMath content dictionaries.
Ultimately, these connections form a new content dictionary base.
This provides multilingual background information for symbols in MathML formulae.

## About this Git Repository

This repository contains the source code of a system description paper, that was submitted to [CICM 2018](https://www.cicm-conference.org/2018/cicm.php).
From this repository you can
* [get the compiled pdf document,](https://github.com/ag-gipp/18CicmWikidata/releases/latest)
* [change abstract, title, authors,](main.tex)
* [add references,](main.bib)
* [or the edit main content.](main.md)


__Warning:__ The first line of every commit message that is committed and the hash commit is submitted to the timestamping service [originstamp](https://originstamp.org/home).
Originstamp combines your commit hash with other information and stores it in the bitcoin blockchain.
While the history of this git repository might be rewritten, the hashes of commits will remain a part of the bitcoin blockchain.



