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

## What is it?

A Cost-Benefit Analysis (CBA) is a **systematic process** for **calculating and comparing** the **monetary costs and benefits** of a project or decision to determine if it is **worthwhile**.

In our project, a CBA involves:
* Quantifying the **economic impact** of immersive digital twin systems in healthcare delivery.
* Identifying both **negative** (costs) and **positive** (benefits) **impacts**.
* **Assigning a monetary value** to these impacts.
* Discounting these future cash flows to a present value to account for the time value of money.
* Calculating metrics like ***Net Present Value*** (NPV), ***Internal Rate of Return*** (IRR), and the ***Benefit-Cost Ratio*** (BCR) to **assess economic viability**.
* Analyzing how results change under **different scenarios and levels of risk.**

## Cost-Benefit Analysis (CBA): Pros, Cons, and Challenges

### Pros (Advantages)

* **Provides a Rational Framework**: CBA offers data-driven method to evaluate project impacts in a more **objective way**.
* **Enables Project Comparison**: By translating all impacts into a common monetary unit, it allows to compare different projects and allocate scarce resources to the one that provides the greatest benefit.
* **Promotes Transparency**: **Assumptions and valuations have to be explicitly stated**, making the process **transparent to stakeholders**.

### Cons (Disadvantages) of CBA

* **Difficulty in Valuing Intangibles**: Assigning a monetary value to non-market goods. 
    * Placing a price on factors like "patient comfort," "learner confidence", or a Quality-Adjusted Life Year (QALY) is inherently difficult and can be controversial.
* **Sensitivity to Assumptions**: Particularly the ***discount rate chosen*** and ***projections about uptake and costs***.
* **Potential for Manipulation**: by **selecting assumptions that favor** a preconceived conclusion.

### Limits and Challenges Faced in a CBA and how to address them.

* **Managing Uncertainty**: dealing with uncertainty about the future. 
    * Our project's success is dominated by risks like **suboptimal user uptake**, **hardware cost volatility**, and the **variance in clinical effectiveness**. 
    * To address this, extensive risk analysis is needed. including scenario analysis, sensitivity analysis, and a Monte Carlo simulation.
* **Monetizing Non-Market Impacts**: Your analysis had to use "hypothetical-market" valuation methods to assign a monetary value to benefits that don't have a market price, such as the social value derived from QALYs or the consumer surplus from access to a digital twin environment. These are necessary estimates but represent a limitation.
* **Ignoring Equity**: A standard CBA can overlook how costs and benefits are distributed across society
    * Our project addressed this by conducting the analysis on ***high-income area***, *Rome*, and ***lower-income province***.

# Specific questions

### If you were a manager, what could be the outcome you could get from this study, what could you implement?**

As a manager, outcome from this study is a clear, investing in immersive digital twins is highly profitable.

Based on the findings, I would implement one of two options:
* **If capital is available**, I would prioritize the **Hospital Simulation & Education Network**. 
    * It delivers the ***largest absolute financial return***.
* **If the budget is more constrained**, I would begin with a phased rollout of **Patient-Specific Virtual Care**. 
    * It is ***more efficient per euro invested***.

### For the implementation (about first question), what should you consider to address it?

The most critical factor to address is managing **user uptake (adoption)**, because the main project's risk.

To address it, as said in our conclusion, I would consider these key actions:
* Do **phased rollout**, led by clinical experts, to prove it works before we go fully invest in the project.
* Negotiate **long-term contracts** with hardware vendors to ***mitigate the risk of price volatility***.
* Use live dashboards to track our results and make sure the benefits are real..

**3. How does the Systematic Review link the literature to address the research question?**

The Systematic Review (SR) is a **bridge between the existing literature and the research question**. 

To achieve this:
* Starts by translating research question into a *searchable format* (***QUERY***) using the **PICO framework**.
* It then uses reproducible **search strategy** to identify all relevant studies.
* Finally, applies **inclusion and exclusion criteria** to filter the literature, ensuring that only the studies that directly address the research question are included in the final **synthesis** of evidence.

**4. What is the reliability of your estimate?**

The reliability of the positive financial estimate is **very high**. 

This is ***not a fragile estimate*** but a consistent finding under **multiple conditions**:
* **Scenario Analysis**: The project NPV **remains positive** even in the most "Pessimistic" scenario
    * Worst Scenario: ***low uptake and high costs***.
* **Break-Even Analysis**: The projects remain **positive** even if with our lowest estimated adoption rates.
* **Monte Carlo Simulation**: Across 20,000 randomized scenarios, the probability of the project having a negative NPV was **0.0%**. 
    * Even the worst 5% of outcomes were still highly profitable.

**5. Is it possible that there is bias in your study?**

While bias is a potential risk in any study, this work used specific methodologies to minimize it and ensure the very positive outcome is a robust finding, not a preconceived one.

* **In the Systematic Review**: 
    * **PRISMA methodology** was used to prevent "***cherry-picking***" positive studies.
* **In the Cost-Benefit Analysis**: Positive result is not based on a single set of optimistic assumptions. 
    * The extensive **uncertainty and risk analysis** (scenarios, Monte Carlo simulations) was specifically designed to avoid bias. 
    * The fact that the NPV remains positive even under pessimistic conditions proves the result is robust and not just an artifact of wishful thinking.

**6. If you were a data scientist that has done this paper, what could you tell about warnings to a policy maker?**

I would offer ***three key warnings*** to a policy maker:

1.  **The benefits are not automatic.** My models show that the positive NPV is highly dependent on achieving the predicted clinical outcomes and high user adoption rates. 
    * I recommend a **staged rollout with pilot programs** to verify impact before large-scale investment.
2.  **A one-size-fits-all policy will be socially inefficient.** The distributional analysis shows that the project's value is higher when implemented in lower-income regions. 
    * Therefore, policy should use **equity-focused** guided investments where it will generate the greatest return, rather than a uniform national subsidy.
3.  **The biggest risk is human, not technological.** My sensitivity analysis clearly shows that **user uptake is the most critical variable** driving the project's success. 
    * Policy must support the technology maintainance, but also the training and choice required to ensure clinicians and patients actually use it.