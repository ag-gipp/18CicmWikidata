# Introduction and Prior Works

Traditionally, mathematical formulae occur in a textual or situational context.
Human readers infer the meaning of formulae from their layout and the context.
An essential task in mathematical information retrieval (MathIR) is to mimic parts of this process to automate MathIR tasks.
There are different approaches to evaluate the effectiveness of MathIR tasks.
The naïve approach is to

1. identify typical information needs,
1. use existing corpora that embed the formula layout in textual content, and
1. evaluate the effectiveness of IR system for the defined information needs with human domain experts.

The advantage of this approach is that it evaluates the practical relevance of real-world applications.
The disadvantages are the following:

1. it is expensive, due to the considerable evaluation effort,
2. it is tailored to the information needs of the task, and
3. it is difficult to identify the reasons for the limited performance of a system.

As an alternative, we suggest breaking down complex MathIR tasks into several subtasks.
The first subtask is to convert formulae from their source representation to a machine-readable format which describes the semantics.
With our MathMLBen project [@ScharpfSG18; @SchubotzGSMCG18], we created an open gold standard for this task. MathMLBen provides a list of over 300 formulae and their textual contexts.
The example formulae include formulae that were used in the NTCIR search tasks [@aizawa2013ntcir; @disNtcir11Ov; @NTCIR12], as well as formulae from the DLMF~[@dlmf] and DRMF~[@disCicm15].
For all formulae, the original LaTeX representation, a corrected LaTeX form (that corrects typographic errors in the layout), and a semantic LaTeX interpretation is given.
From the semantic LaTeX representation, we generate parallel content and presentation MathML markup using LaTeXML [@Miller].
We consider the generated MathML as a first version of a machine-readable format which describes the semantics and use it to measure the effectiveness of different systems for the task described above [@SchubotzGSMCG18].
However, this machine readable format is not yet compatible with the OpenMath standard.
In Section \ref{sc2}, we describe how we generated a first version of the Wikidata content dictionary `wikidata.ocd` contains all symbols occurring in our gold standard.
However, not all symbols of our gold standard were associated with Wikidata items, but with standard OpenMath symbols.
In Section \ref{sc3} we experiment with exchanging those standard symbols with Wikidata items and analyse the effects.
Finally, Section \ref{sc4} concludes the paper and points out future works.

# The first version of a Wikidata content dictionary
\label{sc2}

\input{img1}
As introduced earlier, a key feature of MathMLBen are special LaTeX(ML) macros [@ScharpfSG18].
Those macros link `csymbol`-elements in the MathML to entries in Wikidata.
For example, one element of our gold standard includes the logistic function $f(x) = \frac{L}{1 + \mathrm e^{-k(x-x_0)}}.$
We did not find a symbol for the logistic function in the OpenMath content dictionaries\footnote{A list of all OpenMath symbols is available from \url{https://www.openmath.org/symbols/}.}.
However, articles regarding the logistic function can be found in many different languages in Wikipedia.
Moreover, the Wikidata item Q1052379 connects all these Wikipedia articles.
This item was manually improved by 19 users (excluding bots).
Through these improvements, additional data such as properties, relations to other items and external identifiers were added.
Figure \ref{fg1} shows a screenshot of the item, including statements, external identifiers, and links to Wikipedia articles as well as other Wiki projects such as Wikisource and Wikiversity.
In our gold standard, we used the semantic LaTeX macro\footnote{
  The difference between the macros `\w` and `\wf` is that `\wf` associates the role function with a symbol.
  For example, the first invisible operator in $f(a+b)$ is interpreted as function application rather than multiplication, if `\wf` is used~[@ScharpfSG18].}
 \verb|\wf{Q1052379}{f}| to encode that LaTeXML should treat the symbol $f$ as `<csymbol cd="wikidata">Q1052379</csymbol>`~[@ScharpfSG18].
However, the content dictionary `wikidata` that the `csymbol` element refers to with its `cd` attribute did not exist.  

Currently, there are more than 49 million items in Wikidata.
Not all items are part of mathematical formulae.
Thus, creating a Wikidata content dictionary based on all items would be impractical.
In this paper, we present a Wikidata content dictionary that includes all the 280 entries used in MathMLBen.
We call our content dictionary `wikidata.ocd`.
It can be downloaded from \url{https://cd.formulasearchengine.com/wikidata.ocd}.

\input{lst1}

Listing \ref{ls1} shows the content dictionary entry for the symbol logistic function (Q1052379).
All 280 symbols follow the same pattern.
This is because they generated them with MathTools [@GreinerPetter2018] using the Wikidata item numbers given in the `csymbol`-elements in MathMLBen.
The description includes the English label of the Wikidata entry (line 1366), the English description (not available in Listing \ref{ls1}), a link to the English Wikipedia article (line 1367) and finally a static link to the version of the Wikidata item that was used to create the content dictionary entry (line 1370).
While this description is  currently in English, the language is only a configuration parameter in our tool.
Theoretically, any other language could be used.
However, the success depends on the number of available community-maintained texts in that language (cf. Section~\ref{sc3}).

Our content dictionary `wikidata.ocd` improves the standard compliance of the  MathMLBen gold standard. 
It provides a content dictionary for third party MathML processing software that can read user-contributed content dictionaries without requiring a special implementation to fetch data from Wikidata.
This improves the standard compliance the MathMLBen gold standard. 


# Using Wikidata as cdbase
\label{sc3}

After having discussed how to automatically derive a content dictionary from a set of Wikidata items, we discuss  how to create a cdbase that contains the standard OpenMath symbols from Wikidata in this chapter.
\input{img2}

As Figure \ref{fg2} shows, the nature of content dictionaries and Wikipedia pages (here in English) is different.
While the CD description is brief in human-readable content, the Wikipedia page shows a lot of human-readable information.
On the other hand, there are formal mathematical properties that are hard to extract from the Wikipedia page.
Consequently, we analyse the strength and weaknesses of both approaches in the following.
However, before doing so, we need to map entries in Wikidata to OpenMath.
We therefore proposed a new property in Wikidata (P5610) of type external identifier which was approved on August 9th 2018.
This identifier, labeled OpenMath ID, allows one to refer to OpenMath symbols from within Wikidata.
That way, we connect both communities, Wikidata and OpenMath.
Everyone (even without a Wikidata account) is now able to create new mappings.

The new property P5610 is of type string and has three constraints:
a single-value, a unique value, and a regexp filter `([a-z]+[0-9]*)\#([a-z_]+)` constraint.
These constraints prevent common mistakes.
Table \ref{tb1} lists the 50 standard OpenMath symbols that occur in the MathMLBen project.
We uploaded these manually created mappings on August 10th, 2018 to Wikidata.
Consequently, we now have the opportunity to describe all symbols that occur in the gold standard in terms of Wikidata without referring to any OpenMath definitions. 
One can now create a _redirect service_ which redirects traditional MathML and OpenMath IDs such as `<plus>` which correspond to `arith1#plus` to the associated Wikidata entry (e.g., Q32043 for plus).
According to the MatML standard (Section 4.2.3.2) the URI of a definition can be given as
`URI = cdbase + '/' + cd-name + '#' + symbol-name`.
The SPARQL interface \url{query.wikidata.org} allows one to find an item that is associated with the last part of the url (`cd-name#symbol-name`).
For instance, the query for `arith1#plus` reads `SELECT ?x  WHERE { ?x wdt:P5610 'arith1#plus'.}` which returns the URL \url{http://www.wikidata.org/entity/Q32043}, the Wikidata item for plus.
To materialise the results, we used the method described in Section \ref{sc2} to generate `CDDefintions` for the 50 standard symbols.

\input{lst2}
Listing \ref{ls2} shows an entry in `wikidata.ocd` that corresponds to the Wikidata item plus.
The description section contains more information than Listing \ref{ls1}.
Line 269 is the English description from the item that represents the addition in Wikidata.
Moreover, line 272 links to the definition form of `arith1#plus` from the OpenMath content dictionary.


For the remainder of the section, we discuss the differences between traditional OpenMath symbol definition entries and Wikidata generated symbol definitions.
Our Wikidata content dictionary contains 330 symbols in a single content dictionary.
In contrast, there are 289 official OpenMath symbols divided into 38 content dictionaries.
247 OpenMath symbols have a role attribute (application 193, constant 39, binder 3, semantic-attribution 2, error 3).
While we did research on identifying Wikidata items as numerical constants [@Schubotz2018a], this information is not included in the current version of `wikidata.ocd`.
Moreover, the official OpenMath content dictionaries contain 149 examples, 180 formal mathematical properties (FMP), and 179 commented mathematical properties (CMP).
Currently, `wikidata.ocd` does not contain any of the aforementioned features.
Due to the lack of time, the MathMLBen data has not been converted to the OpenMath XML format, which would be required to create examples.
Deriving reasonable CMP or FMP from Wikidata requires semantic enhancement of the defining formula statement which are currently only available in presentation form.
The description field in the official OpenMath content dictionaries is on average 131 words, as compared to 212 words (including 14 words for the reference to the source) in `wikidata.ocd`.
While Wikidata items are not divided into a structure comparable to content dictionaries, they have hierarchical relations such as the `instance of` (P31) relation.
As displayed in Table \ref{tb1}, the instance of relation is not modelled consistently.
We hypothesise that this is typical for corpora which emerged from community interactions. 
Finally, the symbol names in the standard OpenMath dictionaries are easier to remember for English speakers.
Therefore, using IDE or smart editors is a prerequisite to work with Wikidata items conveniently.
Otherwise, the long numeric item identifiers are hard to read and to remember.
To support this purpose, we released the node module `codemirror-wikidata`~[@Schubotz18c], which provides autocompletion based on the description rather than on the numeric values.

#Conclusions and Future Works
\label{sc4}

In this paper, we released a first version of the Wikidata content dictionary `wikidata.ocd` to the public.
It contains all the symbols used in the MathMLBen open gold standard.
Moreover, it contains 50 entities that correspond to standard OpenMath symbols.
Furthermore, we introduced the new Wikidata property P5610 and described how it can be used to create an alternative cdbase.
Also, we compared symbol descriptions that were generated automatically from Wikidata to the manually crafted OpenMath symbol definitions.
While multilingualism and links to Wikipedia might be considered as an advantage of the Wikidata cdbase, many other formal aspects such as structure and type information are currently better modeled in the traditional OpenMath content dictionaries.
On the other hand, Wikidata has far more items that could be possibly used as symbol definitions.

Future research should investigate how the missing formal information in `wikidata.ocd` can be automatically extracted.
If there was a mechanism to generate content dictionaries from Wikidata that have the same formal quality as the current OpenMath content dictionaries, a good foundation for CD extension, based on Wikidata, would ease the expansion of the OpenMath standard.
Another promising research direction is to better understand how information given in distributed data sources can be connected using alignments~[@KaliszykKMR16;@Mueller17;@MullerGKKR17].
