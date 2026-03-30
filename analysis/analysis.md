### Root Cause Distribution Across Tasks

Finally, we analyze the differences in failure root cause distribution from the perspective of **system tasks**. For clarity, we categorize the ten systems into four task groups:

- **Software Development** (code repair, code generation)  
- **Task Planning** (travel assistant)  
- **Question & Answering**  
- **Information Insight** (deep research, industry analysis, marketing research)  

This categorization allows us to examine whether different application domains and system objectives are associated with **distinctive failure patterns**.

The radar chart (the following figure) reveals that different tasks exhibit clearly differentiated failure root cause modes.

- **Software development** systems most frequently fail due to *knowledge or reasoning limitations* and *poor prompt design*, reflecting their heavy reliance on LLMs for code generation and validation.

- **Question answering** tasks are more prone to *response format errors* and *tool invocation or retrieval errors*, as these tasks often require strict output formats and precise tool usage.

- **Task planning** systems show a higher prevalence of *language or encoding issues* and *unreasonable node dependencies*, highlighting their sensitivity to control flow and dependency management.

- **Information insight** tasks, which typically involve multi-step reasoning and synthesis, are dominated by *response content deviation* and *reasoning limitations*, suggesting that subtle semantic errors tend to accumulate across long reasoning chains.


![task](../fig/task.png)