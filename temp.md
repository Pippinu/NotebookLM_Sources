# Systematic review
## What is it?
A systematic review is a rigorous and transparent research method used to identify, evaluate, and synthesize all available evidence that fits pre-specified eligibility criteria to answer a specific research question.

## What method in our work?
Our project followed the **PRISMA guidelines** to conduct the SR, which follows a flow about studies on **how to**:
* Eligibility of studies
* Screening and inclusion of studies
* Extraction and synth of evidence in studies

## What is the PICO framework?

The PICO framework is a mnemonic used in evidence-based practice to frame and answer a clinical or healthcare-related question. It helps structure the research question to make it more specific and answerable.

As described in your paper, the components are:
* **P** - **Population**: Who are the relevant patients or subjects? 
    * (e.g., medical professionals, students, or patients).
* **I** - **Intervention**: What is the new treatment, technology, or procedure being considered? 
    * (e.g., digital twin-enhanced metaverse technologies).
* **C** - **Comparator**: What is the alternative or current standard of care? 
    * (e.g., conventional methods).
* **O** - **Outcomes**: What are the desired results or effects? 
    * (e.g., improved clinical training, health monitoring, or treatment outcomes).
* **Reasons, why conduct a SR?**:
    * Identify state of art in a specific field.
    * Examining methods and results from previous studies.
    * Analyzing studies for possible biases and errors.
* **Characteristics of SR**:
    * **Reproducible** methodology.
    * Pre-defined **eligibility criteria**.
    * Systematic **research for eligible studies**.
    * Assestment of **validity of findings**.
    * **Systematic presentation of findings** about included studies.

## Stages of SR
1.  **Formulate a Research Question**: Start with a clear, specific, and answerable question that will guide the entire review. 
    * Your study used the PICO framework for this.
2.  **Develop a Protocol**: Plan the methods in advance, including the search strategy and the inclusion/exclusion criteria, to ensure a transparent and unbiased process.
3.  **Conduct a Comprehensive Search**: Systematically search multiple databases to find all relevant studies.
    * We used PubMed and Scopus
4.  **Screen and Select Studies**: Apply the inclusion/exclusion criteria to the search results to identify the final set of studies for the review. 
    * Documented in **PRISMA flow diagram** and used ***Rayyan*** tool.
5.  **Extract Data**: Systematically pull the relevant data from each included study in a consistent manner.
6.  **Assess the Quality of Studies**: Evaluate the **quality** and **risk of bias** of the included studies.
7.  **Synthesize, Interpret, and Report**: Combine the extracted data to answer the research question. 
    * Your study did this by extracting "thematic clusters". 

The final step is to write the review, presenting the results and conclusions, often using the PRISMA checklist to ensure complete reporting.

## Difference between SR and Bibliometric Analysis?

While ***both analyze literature***, they do so with different goals and methods.

* **Systematic Review** (SR): Aims to answer a specific research question by synthesizing the content and findings of included studies. 
    * Its focus is on the evidence within the papers.

* **Bibliometric Analysis**: Analyzes the metadata of publications (like keywords, authors, citations, and publication dates) to map the structure, evolution, and major trends of a research field. 
    * Its focus is on the characteristics of the literature itself.
    * We used ***bibliometrix R package*** and **Orande Data Mining** preprocessing tool.

**In your project**, you used a bibliometric analysis as a tool within the early stages of your systematic review to understand the research landscape and refine your search strategy

## SR pro and cons:

### Pros (Advantages)

* **High Level of Evidence**: A well-conducted SR is considered one of the highest levels of evidence because it gathers and synthesizes findings from multiple studies.
* **Reduces Bias**: The use of a strict and predefined protocol (like PRISMA) minimizes the bias that can affect traditional literature reviews.
* **Comprehensive and Reproducible**: The methodology is designed to be exhaustive and clearly documented, which means other researchers can replicate the process to verify the findings.
* **Identifies Research Gaps**: By systematically mapping all available evidence, an SR is an excellent tool for identifying areas where more research is needed, as your own study did[cite: 52].
* **Informs Policy and Practice**: The strong conclusions drawn from SRs are often used to create clinical practice guidelines and inform healthcare policy.

### Cons (Disadvantages)

* **Resource-Intensive**: SRs require a significant investment of time, personnel, and resources to conduct properly due to the rigorous and comprehensive process.
* **Dependent on Existing Research**: The quality of the SR is entirely dependent on the quality of the studies it includes. If the available literature is of poor quality, the SR's conclusions will be weak (the "garbage in, garbage out" principle).
* **Risk of Publication Bias**: SRs can be skewed by publication bias, where studies with statistically significant or positive results are more likely to be published and found than those with negative or inconclusive results.
* **Narrowly Focused Question**: The need for a highly specific research question means that an SR might not capture the broader context of a topic.
* **Challenges with Heterogeneity**: It can be difficult to synthesize or compare findings if the included studies are very different from one another (e.g., in their methods, populations, or outcome measures).

* **Challenges encountered in our SR?** (ABOUT OUR STUDY)
    * **Limited High-Quality Evidence**: A significant challenge is the **scarcity of large-scale clinical validation** for these technologies. 
        * Your review notes a "clear disconnect between the high technological potential of AR/VR and the currently limited clinical evidence".

    * **Inconsistent Efficacy**: The **benefits are not uniform across all applications**. 
        * While results are strong in surgical planning, the efficacy in areas like post-stroke rehabilitation is more variable and less consistent.

    * **Scalability and Practical Barriers**: There are major practical hurdles to widespread adoption. 
        * Your paper highlights challenges such as:
            * high costs
            * the lack of professional training
            * data security concerns
            * issues with device interoperability 
            
            that hinder integration into real-world.

# CBA