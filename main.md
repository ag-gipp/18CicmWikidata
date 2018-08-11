# Introduction and prior works 

Traditionally, mathematical formulae occur in a textual or situational context. Human readers infer the meaning of formulae from their layout given a context. An essential task in mathematical information retrieval (MathIR) is to mimic parts of this process to automate MathIR tasks. There are different approaches to evaluate the effectiveness of MathIR tasks. The naïve approach is to (1) identify typical information needs, (2) use existing corpora that embed the formula layout in textual content, and (3) evaluate the effectiveness of IR system for the defined information needs with human domain experts. The advantage of this approach is that it evaluates the practical relevance of real-world applications. The disadvantages are the following: (1) this approach it is expensive, due to the considerable evaluation effort, (2) it is tailored to the information needs of the task, and (3) it is difficult to understand the reasons for the performance of the systems. As an alternative, we suggest breaking down complex MathIR tasks into several subtasks. The first subtask is to convert the formulae from its source representation to a machine-readable format that describes the semantics. With our MathMLBen project, we created an open gold standard for this task. MathMLBen provides a list of over 300 formulae and their textual contexts. The example formulae include formulae that were used in the NTCIR search tasks as well as formulae from the DLMF. For all formulae there is the original LaTeX representation, a corrected LaTeX form (that corrects typographic errors in the layout) and a semantic LaTeX interpretation. From the semantic LaTeX representation, we generate parallel content and presentation MathML markup using LaTeXML. We consider the generated MathML as a first version of a machine-readable format that describes the semantics and use it to measure the effectiveness of different systems for the task described before. 

# The first version of a Wikidata Content Dictionary 

\input{img1}

A key feature our semantic LaTeX macros is that we linked symbols in the MathML to entries in Wikidata.
For example, we did not find a symbol for the logistic function $f(x) = \frac{L}{1 + \mathrm e^{-k(x-x_0)}}$ in the OpenMath content dictionaries.
However, there are articles in many different languages in Wikipedia regarding the logistic function.
Moreover, the Wikidata item  which connects all the Wikipedia articles in different version and was edified by 19 users excluding bots.
Figure \ref{fg1} shows a screenshot of the wikidata item, including statements, external identifiers, and links to Wikipedia articles and other Wiki projects such as Wikisource Wikiversity that address the topic Logisitic function.
In our gold standard we used the semantic LaTeX macro\footnote{
  The difference beteen the macros `w` and `wf` is that `wf` associates the role function with a symbol.
  For example, the first invisible operator in $f(a+b)$ is interpreted as function application rather than multiplication ,if `wf` is used.}
 \verb|\wf{Q1052379}{f}| to encode that LaTeXML should treat the symbol $f$ as \verb|<csymbol cd=wikidata>Q1052379</csymbol>|.
This example shows that the use IDE or smart editors is a prerequisite to work with wikidata Symbols conveniently, since long numberic item identifiers are hard to read and to remember.
To supp this purpose, we released the codemirror-wikidata plugin which provides autocompletion based on the description rather than on the numeric values. 
Currently, there are more than 49 million items in Wikidata.
Not all items are part of mathematical formulae.
In this paper, we present a wikidata content dictionary with 330 entries.
It includes all entries that were created as part of the MathMLBen project, which we call `wikidata.ocd`.
  
\input{lst1}

Listing \ref{ls1} shows the content dictionary entry for the symbol logistic function (Q1052379).
The description was automatically created with MathTools from Wikidata.
It includes the english label of the wikidata entry (line 1366), the english description (not available in Listing \ref{ls1}), a link to the english Wikipedia article (line 1367) and finally a static link to the version of the Wikidata item that was used to create the content dictionary entry (line 1370).
After converting all valid formulae in MathMLBen to strict content MathML using MathTools we obtained 4529 symbols.
The most frequent symbols included complex and natural numbers and subclasses such as positive natural numbers.
However, the main advantage of using Wikidata is the large variety of different symbols. 

# Using wikidata as cdbase

After having discussed how to automatically derive a content dictionary from a set of Wikidata item, we introduction the create of a Wikidata based content dictionary base in this chapter.
\input{img2}
As Figure \ref{fg2} shows, the nature of content dictionaries and Wikipedia pages (here in englsh) is quite different. While the CD description is brief in human readable content, the Wikipdia page sows a lot human readable information.
On the other hand, there are formal mathematical properties that are hard to extract from the Wikiedpia page. Consequently, we combine the strengths of both approaches.
\input{lst2}
Listing \ref{ls2} shows an entry in `wikidata.ocd` that corresponds to the wikidata item plus.
The description is longer compared to Listing \ref{ls1}.
Line 269 is the english description from the item that represents the the addition in wikidata.
Moreover, line 272 links to the definition from of `arith1#plus` from the OpenMath content dictionary.
We proposed a new property in Wikidata (P5610) which is an external identifier.
This identifier labels OpenMath ID allows to refere to OpenMath sysmbols from within Wikidata.
That way, we connect both worlds Wikidata and OpenMath.
Instead of mixing content symbols from Wikidata and OpenMath, one can now create a redirect sevice that redirects traditional MathML and OpenMath Ids such as `<plus>` which corresponds to `arith1#plus` with the associated Wikidata entry.
Table \ref{tb1} list 50 of those connections.  

#Conclusions and Future Works

In this paper, we presented a collection of Wikidata entries and developed an alternative to the standard openmath content dictionary group.
By doing so canonical symbols in MathML can be linked to 
