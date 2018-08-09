# Introduction and prior works 

Traditionally, mathematical formulae occur in a textual or situational context. Human readers infer the meaning of formulae from their layout given a context. An essential task in mathematical information retrieval (MathIR) is to mimic parts of this process to automate MathIR tasks. There are different approaches to evaluate the effectiveness of MathIR tasks. The naïve approach is to (1) identify typical information needs, (2) use existing corpora that embed the formula layout in textual content, and (3) evaluate the effectiveness of IR system for the defined information needs with human domain experts. The advantage of this approach is that it evaluates the practical relevance of real-world applications. The disadvantages are the following: (1) this approach it is expensive, due to the considerable evaluation effort, (2) it is tailored to the information needs of the task, and (3) it is difficult to understand the reasons for the performance of the systems. As an alternative, we suggest breaking down complex MathIR tasks into several subtasks. The first subtask is to convert the formulae from its source representation to a machine-readable format that describes the semantics. With our MathMLBen project, we created an open gold standard for this task. MathMLBen provides a list of over 300 formulae and their textual contexts. The example formulae include formulae that were used in the NTCIR search tasks as well as formulae from the DLMF. For all formulae there is the original LaTeX representation, a corrected LaTeX form (that corrects typographic errors in the layout) and a semantic LaTeX interpretation. From the semantic LaTeX representation, we generate parallel content and presentation MathML markup using LaTeXML. We consider the generated MathML as a first version of a machine-readable format that describes the semantics and use it to measure the effectiveness of different systems for the task described before. 

# The first version of a Wikidata Content Dictionary 

A key feature our semantic LaTeX macros is that we linked symbols in the MathML to entries in Wikidata.
For example, we did not find a symbol for the logistic function $f(x) = \frac{L}{1 + \mathrm e^{-k(x-x_0)}}$
19 edits

semantic LaTeX macro \verb|\wf{Q190573}{\Gamma}| would associate the Wikidata entry for the gamma function with




