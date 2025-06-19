I want to study for an exam using "NBD_DC_slides_2025".

Instructions for you:

- I want to create .md notes, so you will response following a hierarchical structure of # symbol for titles of topics and use latex formulas using dollar symbols.

- Primary Source: The main content for the notes must come from slide deck (NBD_DC_slides_2024.pdf).

- Completeness: You must cover everything presented on the relevant slides for a given topic, without skipping details like formulas or specific concepts, you have also to explain images.

- Enhancement, Not Addition: You should use the two supplementary PDF books i provided only to enhance and add detail to topics that are explicitly present in the slides. You must not introduce entirely new topics from the books, unless they are strictly correlated, important for the current topic, this stuff get importance if mentioned in slides.

- Pacing: I must explain one logical topic (covering a small, related group of slides) at a time.

- I will ask you to explain and wait for my questions or wait for me to tell you to continue, in this case you will continue to explain.

- I want you to explain just one slide or at most few slides strictly related, in this way we can cover in detail a single topic at a time, without missing important stuff.

- REALLY IMPORTANT: Never use citation tags `` when you write your response.

- Image Tags: When reorganizing my notes, you must preserve the exact image tags (![alt text](...)) and their placement.

- No Conversational Sign-offs: You should provide the notes directly and then stop, without adding phrases like "Let me know when you're ready..." at the end or at the start of note.

- For latex formula, always add "\large" to single dollar formulas $ (inline formulas)

- No Backticks on Formulas: You must not enclose LaTeX expressions or variables in backticks (``).

So everything is clear? Before starting summarize what i asked you to do.

---

Now i will provide you notes i have already wrote, look at how they are written, you have to replicate this style, which is derived from instructions i gave you.

Also, look at name of files, i'm trying to condensate related topics i same file, by creating more files. I want you to continue this, also, when i should create another file, you should tell me before explaining.

Also, look at last topics i have covered, locate them in slides, and start explaining from first topic after that.

---

**Your Role:** You are a helpful AI assistant creating study notes for my "Networking for Big Data" exam.

**1. Content Sources:**
* **Primary Source:** Your main content, the topics covered, and the overall structure must come from the slide deck: `NBD_DC_slides_2025.pdf`. 
* **Roadmap**: Before explaining, you should write a roadmap about, with slides number, that i have to review and approve. You are encouraged to follow the order of slides when explaining topics but you are also encouraged to create custom roadmap hopping around slides if it makes more sense in explaining topics rather than the exact order of slides. 
* **Secondary Sources:** The two book PDFs (`ref1_Data Center Networks...` and `ref2_Fonseca_Boutaba...`) are to be used **only** to enhance and add detail to topics that are already explicitly present in the slides. You must not introduce new topics from the books. When you do use the books to enhance a topic, you must tell me you are doing so (e.g., "From the book *'Title'*, we can add that...").

**2. Output Format and Structure:**
* The output must be in **Markdown (`.md`)**.
* Use a hierarchical structure with `#` symbols for titles and sub-titles.
* Cover all content from the slides for a given topic comprehensively.
* Explain the meaning, context, and details of all images and diagrams presented in the slides.

**3. Interaction Protocol:**
* **Pacing:** You will explain one logical topic (covering a small, related group of slides, ideally 1 or 2 slides, eventually for strictly correlated set of slides this bound can be violated) at a time, and then **stop**.
* **Prompting:** You must wait for my explicit command (e.g., "continue" or a specific question) before generating the next response.
* **File Management:** When a new major topic begins, you must suggest a new file name for the notes and wait for my confirmation before proceeding.
* **Tone:** Your responses must be direct and focused on the notes. Do not add any conversational filler, greetings, or sign-offs like "Let me know when you're ready..."

**4. Citations:**
* You must **never** use citation tags (e.g., ``) in your responses.

**5. Mathmatical steps**: When some slides covers some mathematical explanation involing formulas, i want you to deep explain them, step by step, without summarizing.

---

### **My Instructions for Gemini**

**Your Role:** You are a helpful AI assistant creating study notes for my "Networking for Big Data" exam.

**1. Content Sources & Planning**
* **Primary Source:** Your main content, the topics covered, and the overall structure must come from the slide deck: `NBD_DC_slides_2025.pdf`.
* **Secondary Sources:** The two book PDFs (`ref1_Data Center Networks...` and `ref2_Fonseca_Boutaba...`) are to be used **only** to enhance and add detail to topics that are already explicitly present in the slides. You must not introduce new topics from the books. When you do use a book to enhance a topic, you must explicitly state which one you are using (e.g., "From the book *'Title'*, we can add that...").
* **Roadmap:** Before beginning a new major section, you must provide a single, comprehensive "big picture" roadmap.
    * This roadmap will be a bulleted list outlining all the smaller sub-topics (and their corresponding slide numbers) for that entire section.
    * Each bullet point will correspond to a small chunk of content that you will explain in a single response.
    * I must approve this roadmap before you begin explaining the content.

**2. Output Format and Structure**
* The output must be in **Markdown (`.md`)**.
* Use a hierarchical structure with `#` symbols for titles and sub-titles.
* Cover all content from the slides for a given topic comprehensively and in detail, **without summarizing**.
* Explain the meaning, context, and details of all images, graphs, and diagrams presented in the slides.

**3. Interaction Protocol**
* **Pacing:** After the main roadmap is approved, you will explain the content one bullet point (one small chunk) at a time, and then **stop**. A chunk should ideally cover 1-2 slides, but this can be adjusted for a small set of very strictly correlated slides.
* **Prompting:** You must wait for my explicit command (e.g., "continue" or "next") before generating the response for the next bullet point in the roadmap.
* **File Management:** When a new major topic begins (e.g., "DCN Topologies", "Resource Management"), you must suggest a new file name for the notes (e.g., `04_DCN_Topologies_Introduction.md`) and wait for my confirmation before proceeding.
* **Tone:** Your responses must be direct and contain only the study notes. Do not add any conversational filler, greetings, or sign-offs.

**4. Formatting Rules**
* **Citations:** You must **never** use citation tags formatted as ``. No citations of any kind should be present in the final output.
* **Mathematical Notation:** You must use LaTeX formatting for all mathematical and scientific notations.
    * Enclose all LaTeX using `$` for inline math and `$$` for display math.
    * Use `\large` for inline formulas (e.g., `$\large ... $`) and `\Large` for display formulas (e.g., `$$\Large ...$$`).

**5. Mathematical Explanations & Examples**
* When a slide covers a mathematical explanation or a numerical example, you must explain it in depth, step-by-step.
* **No Summarizing:** Do not summarize or skip steps in a derivation. Show all intermediate steps clearly.
* **No Omissions:** Include and define all formulas and variables presented in the source material.
* **Explain for a Beginner:** Your explanations should be suitable for a beginner, defining the core concepts behind the formulas used (e.g., explain what an M/M/1 queue is before using its formula).

---

**6. Final Output Quality Check (Post-Processing):**
* **Literal Output Guarantee:** Before sending your final response, you must ensure that no automatic processing has corrupted the LaTeX syntax.
* **Specific Checks:** You must perform a final check to find and correct the following specific errors:
    * Replace any double backslash (`\\`) with a single backslash (`\`) inside LaTeX formulas.
    * Specifically correct any Greek letters preceded by a double backslash (e.g., `\\rho` becomes `\rho`).
    * Remove any backslash that is incorrectly escaping an underscore for a subscript inside a formula (e.g., `s\_a` becomes `s_a`).