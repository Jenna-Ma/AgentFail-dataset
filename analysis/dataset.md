
### Workflow Structures

Agentic workflows can be organized under different architectural paradigms, each reflecting a distinct way of coordinating agents and tools:

1. **Serial**: Tasks are executed in a sequential manner, where the output of one node serves directly as the input to the next. This design is simple and interpretable but suffers from error propagation, as mistakes in early steps cascade downstream.
![serial](../fig/serial.png)
2. **Parallel**: Multiple nodes process subtasks simultaneously, and their outputs are later aggregated. This improves efficiency and robustness but can lead to synchronization issues or inconsistencies in the merged results.
![parallel](../fig/parallel.png)
3. **Branching**: The workflow splits into different paths based on conditions or task types. While this design enables flexibility and specialization, it may suffer from dependency mismatches or logic errors across branches.
![branching](../fig/branching.png)
4. **Looping**: Certain steps are repeated until a stopping condition is satisfied, enabling refinement and self-correction. However, this design is prone to infinite loops or redundant computation.
![loop](../fig/loop.png)
5. **Hybrid**: Real-world workflows often combine multiple patterns, such as sequential backbones with parallel or looping substructures. Hybrid designs increase flexibility and expressiveness but also complicate debugging and error attribution.
![hybrid](../fig/hybrid.png)

### Multi-LLM Judge 

In evaluating system executions, we draw inspiration from the LLM-as-a-Judge paradigm, where large language models are employed to assess the correctness of outputs in the absence of deterministic ground truth. To mitigate potential bias from relying on a single model, we further adopted a multi-LLM judging strategy, using several independent models to provide parallel assessments. The final decision was derived through majority voting across models. Specifically, we employed GPT-4o, Claude 3.7, DeepSeek, and Gemini, selected for their strong reasoning and complementary capabilities. This ensemble of models ensures greater robustness and helps mitigate systematic biases inherent to any individual model. Example of the prompt we design is as following:

![judge](../fig/prompt-judge.png)


### Test data construction
To ensure the diversity and representativeness of the test input set, we systematically construct test cases based on the following four dimensions:
#### 1. Functional Path Coverage
Based on the official documentation of the workflow and the node topology, we identify all key functional paths.  
Test inputs are designed to trigger different conditional branches—for example, in the travel planning assistant, we test **single-destination**, **multi-destination**, and **invalid-destination** requests—to ensure coverage of:

- Main execution logic  
- Edge cases  
- Error-handling paths  

---

#### 2. Input Mode and Complexity
We consider multiple input modalities and levels of complexity, including:

- **Text complexity**  
  - From simple keyword-based queries  
  - To complex long-form inputs containing multi-round dialogue context  

- **Degree of structuring**  
  - Unstructured natural language input  
  - Partially structured input (e.g., lists)  
  - Fully structured input (e.g., conforming to a specific JSON Schema)  

---

#### 3. Data Augmentation
For agentic systems equipped with knowledge bases (e.g., question-answering assistants), we perform systematic semantic transformations on original knowledge-base questions to generate test inputs that are semantically equivalent but linguistically diverse.

This strategy:
- Expands the scale of the test set  
- Evaluates the system’s robustness to real-world language diversity  

##### Conversion Strategies

- **Lexical and syntactic transformation**  
  - Use pre-trained language models (e.g., Sentence-BERT) to retrieve synonyms  
  - Modify active/passive voice and word order  
  - Example:  
    - *“How long is the battery life of this mobile phone?”*  
    - → *“How long can the battery of this mobile phone last?”*

- **LLM-based semantic generation**
  To efficiently generate large-scale, high-quality semantic variants, we adopt an LLM as the semantic engine:

  - **Seed inputs**  
    - Original questions from the knowledge base  

  - **Instruction-controlled generation**  
    - Structured prompts such as:
      - “Rephrase this question in three different styles”
      - “Simulate novice vs. expert phrasing”
      - “Embed the question in a dialogue context”

  - **Automated filtering and deduplication**  
    - Cluster generated candidates by embedding similarity  
    - Filter redundant or low-quality samples to ensure diversity


### Example of community issue report

![community](../fig/community.png)


### Annotation
Figure 1 shows the proportion of uncertain annotations to total annotations during the first round. Figure 2 shows the initial disagreement rates between annotators (note that we make sure to reach a consensus through a careful discussion and voting process afterwards). 

<p align="center">
  <img src="../fig/uncertainty.png" width="45%" />
  <img src="../fig/disagree.png" width="45%" />
</p>
