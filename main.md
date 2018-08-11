# Introduction and Prior Works

Traditionally, mathematical formulae occur in a textual or situational context.
Human readers infer the meaning of formulae from their layout given a context.
An essential task in mathematical information retrieval (MathIR) is to mimic parts of this process to automate MathIR tasks.
There are different approaches to evaluate the effectiveness of MathIR tasks.
The naïve approach is to
  (1) identify typical information needs,
  (2) use existing corpora that embed the formula layout in textual content, and
  (3) evaluate the effectiveness of IR system for the defined information needs with human domain experts.
The advantage of this approach is that it evaluates the practical relevance of real-world applications.
The disadvantages are the following:
  (1) it is expensive, due to the considerable evaluation effort,
  (2) it is tailored to the information needs of the task, and
  (3) it is difficult to understand the reasons for the performance of the systems.
As an alternative, we suggest breaking down complex MathIR tasks into several subtasks.
The first subtask is to convert the formulae from its source representation to a machine-readable format that describes the semantics.
With our MathMLBen project [@SchubotzGSMCG18; @ScharpfSG18], we created an open gold standard for this task. MathMLBen provides a list of over 300 formulae and their textual contexts.
The example formulae include formulae that were used in the NTCIR search tasks [@aizawa2013ntcir; @disNtcir11Ov; @NTCIR12] as well as formulae from the DLMF~[@dlmf] and DRMF~[@disCicm15].
For all formulae there is the original LaTeX representation, a corrected LaTeX form (that corrects typographic errors in the layout) and a semantic LaTeX interpretation.
From the semantic LaTeX representation, we generate parallel content and presentation MathML markup using LaTeXML [@Miller].
We consider the generated MathML as a first version of a machine-readable format that describes the semantics and use it to measure the effectiveness of different systems for the task described before [@SchubotzGSMCG18].
However, this machine readable format is not yet compatible with the OpenMath standard.
In Section \ref{sc2}, we describe how we generated a first version of the Wikidata content dictionary `wikidata.ocd` that contains all symbols occurring in our gold standard.
However, not all symbols of our gold standard were associated with Wikidata items, but with standard OpenMath symbols.
In Section \ref{sc3} we experiment with exchanging those standard symbols with wikidata items and analyse the effects.
Finally, Section \ref{sc4} concludes the paper and points out future works.

# The first version of a Wikidata Content Dictionary
\label{sc2}

\input{img1}
As introduced earlier, a key feature of MathMLBen are special LaTeX(ML) macros [@ScharpfSG18].
Those macros link `csymbol`-elements in the MathML to entries in Wikidata.
For example, one element of our gold standard includes the logistic function $f(x) = \frac{L}{1 + \mathrm e^{-k(x-x_0)}}.$
We did not find a symbol for the logistic function in the OpenMath content dictionaries.
However, there are articles in many different languages in Wikipedia regarding the logistic function.
Moreover, the Wikidata item Q1052379 connects all these Wikipedia articles.
The item was manually improved by 19 users (excluding bots).
Through these improvements additional data such as properties, relations to other items and external identifiers were added.
Figure \ref{fg1} shows a screenshot of the item, including statements, external identifiers, and links to Wikipedia articles and other Wiki projects such as Wikisource, Wikiversity.
In our gold standard, we used the semantic LaTeX macro\footnote{
  The difference between the macros `w` and `wf` is that `wf` associates the role function with a symbol.
  For example, the first invisible operator in $f(a+b)$ is interpreted as function application rather than multiplication, if `wf` is used~[@ScharpfSG18].}
 \verb|\wf{Q1052379}{f}| to encode that LaTeXML should treat the symbol $f$ as `<csymbol cd="wikidata">Q1052379</csymbol>`~[@ScharpfSG18].
However, the content dictionary `wikidata` that the `csymbol` element refers to with its `cd` attribute did not exist.  

Currently, there are more than 49 million items in Wikidata.
Not all items are part of mathematical formulae.
Thus creating a wikidata content dictionary based on all items would be impractical.
In this paper, we present a wikidata content dictionary that includes all the 280 entries that we use in MathMLBen.
We call our content dictionary `wikidata.ocd`.
It can be downloaded from \url{https://cd.formulasearchengine.com/wikidata.ocd}.

\input{lst1}

Listing \ref{ls1} shows the content dictionary entry for the symbol logistic function (Q1052379).
All 280 symbols follow the same pattern, as we generated them with MathTools [@GreinerPetter2018] from the Wikidata item numbers given in the `csymbol`-elements in MathMLBen.
The description includes the English label of the Wikidata entry (line 1366), the English description (not available in Listing \ref{ls1}), a link to the english Wikipedia article (line 1367) and finally a static link to the version of the Wikidata item that was used to create the content dictionary entry (line 1370).
While this description is in currently in english, the language is only a configuration parameter in our tool.
Theoretically, we could use any other language.
However, the success depends on the number of available community maintained texts in that language (cf. Section~\ref{sc3}).

`wikidata.ocd` improve the standard compliance of the  MathMLBen gold standard. 
It provides a content dictionary for third party MathML processing software that can read user contributed content dictionaries whithout to require a special implementation to fetch data from Wikidata.
This improves the standard compliance the MathMLben gold standard. 


# Using Wikidata as cdbase
\label{sc3}

After having discussed how to automatically derive a content dictionary from a set of Wikidata items, we discuss in this chapter how to create a cdbase that contains the standard OpenMath symbols from Wikidata.
\input{img2}

As Figure \ref{fg2} shows, the nature of content dictionaries and Wikipedia pages (here in English) is different.
While the CD description is brief in human readable content, the Wikipdia page shows a lot human readable information.
On the other hand, there are formal mathematical properties that are hard to extract from the Wikiedpia page.
Consequently, we analyse the strength and weaknesses of both approaches in the following.
However, before doing so we need to map entries in Wikidata to OpenMath.
We therefore proposed a new property in Wikidata (P5610) of type external identifier which was approved on August 9th 2018.
This identifier labeled OpenMath ID allows to refer to OpenMath symbols from within Wikidata.
That way, we connect both worlds Wikidata and OpenMath.
Everyone (even without a wikidata account) is now able to create new mappings.
The new property P5610 is of type string and has a single-value, unique value, and a regexp filter `([a-z]+[0-9]*)\#([a-z_]+)` constraints that prevent common mistakes.
Table \ref{tb1} list the 50 standard OpenMath symbols that occur in the MathMLBen project.
We uploaded these manually created mappings on August 10th 2018 to Wikidata.
Now, we have the opportunity to describe all symbols that occur in the gold standard in terms of Wikidata without to refer to any OpenMath definitions. 
One can now create a _redirect service_ that redirects traditional MathML and OpenMath IDs such as `<plus>` which corresponds to `arith1#plus` to the associated Wikidata entry (e.g., Q32043 for plus).
According to the MatML standard (Section 4.2.3.2) the URI of a definition can be given as
`URI = cdbase + '/' + cd-name + '#' + symbol-name`.
The SPARQL interface \url{query.wikidata.org} allows one to find item that is associated with the last part of the url (`cd-name#symbol-name`).
For instance the query for `arith1#plus` reads `SELECT ?x  WHERE { ?x wdt:P5610 'arith1#plus'.}` which returns the URL \url{http://www.wikidata.org/entity/Q32043}, the Wikidata item for plus.
To materialize the results we used the method described in Section \ref{sc2} to generate `CDDefintions` for the 50 standard symbols.

\input{lst2}
Listing \ref{ls2} shows an entry in `wikidata.ocd` that corresponds to the wikidata item plus.
The description section contains more information than to Listing \ref{ls1}.
Line 269 is the English description from the item that represents the the addition in Wikidata.
Moreover, line 272 links to the definition from of `arith1#plus` from the OpenMath content dictionary.


For the remainder of the section, we discuss the differences between traditional OpenMath symbol definitions entries and Wikidata generated symbol definitions.
Our wikidata content dictionary contains 330 symbols in a single content dictionary.
In contrast there are 289 official OpenMath symbols divided into 38 content dictionaries.
247 OpenMath symbols have a role attribute (application 193, constant 39, binder 3, semantic-attribution 2, error 3).
While we did research on identifying wikidata items as numerical constants [@Schubotz2018a], this information is not included in the current version of wikidata.ocd.
Moreover, the official OpenMath content dictionaries contain 149 examples, 180 formal mathematical properties (FMP), and 179 commented mathematical properties (CMP).
Currently, wikidata.ocd does not contain any of the aforementioned features.
Due to the lack of time the MathMLben data has not been converted to the OpenMath XML format which would be required to create examples.
Deriving reasonable CMP or FMP from Wikidata requires semantic enhancement of the defining formula statement that are currently available in presentation form.
The description field in the official OpenMath content dictionaries is in average 131 words, compared to 212 words (including 14 words for the reference to the source) in wikidata.ocd.
While Wikidata items are not divided into a structure comparable to content dictionaries, they have hierarchical relations such as
the _instance of_ (P31) relation.
As displayed in Table \ref{tb1} the instance of relation is not modelled consistently.
We hypothesise that this typical for corpora that emerged by community interactions. 
Finally the symbol names in the standard OpenMath dictionaries are easier to remember for english speakers.
This motivates that using IDE or smart editors is a prerequisite to work with Wikidata items conveniently.
Otherwise the long numeric item identifiers are hard to read and to remember.
To support this purpose, we released the node module `codemirror-wikidata`~[@Schubotz18c] which provides autocompletion based on the description rather than on the numeric values.

#Conclusions and Future Works
\label{sc4}

In this paper, we released a first version of the Wikidata content dictionary `wikidata.ocd` to the public.
It contains all the symbols we use in the MathMLBen open gold standard.
Moreover, it contains 50 entities that correspond to standard OpenMath symbols.
Furthermore, we introduced the new Wikidata property P5610 and how it can be used to create an alternative cdbase.
Also, we compared symbol descriptions that were generated automatically from wikidata to the manually crafted OpenMath symbol definitions.
While multilingualism and links to Wikipedia might be considered as an advantage of the wikidata cdbase many other formal aspects such as structure and type information are currently better modeled in the traditional OpenMath content dictionaries.
On the other hand, Wikidata has far more items that could be possibly used as symbol definitions.

Future research should investigate how the missing formal information in `wikidata.ocd` can be automatically extracted.
If there was a mechanism to generate content dictionaries from wikidata that have the same formal quality as the current OpenMath content dictionaries, a good foundation for CD extension based on Wikidata would ease the expansion of the OpenMath standard.

Another promising research direction is to understand better how information given in distributed data sources can be connected using alignments~[@KaliszykKMR16;@MullerGKKR17;@Mueller17].
