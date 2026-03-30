## Background

### Workflow Node Elements

The detailed descriptions of workflow node elements are as follows:

#### 1. Start
The **Start** node is a preset node required for every workflow application. It provides the initial information necessary for subsequent workflow nodes and for the normal execution of the application, such as user input content and uploaded files.

---

#### 2. End
The **End** node defines the final output of a workflow execution.  
Each workflow must include at least one End node to output the final result. The End node serves as the termination point of the workflow, and no nodes can be added after it.

If conditional branching occurs, multiple End nodes may be required. Each End node must declare one or more output variables, which can reference the output variables of any upstream node.

---

#### 3. Answer
The **Answer** node allows flexible definition of reply formats through a text editor. It supports:
- Fixed custom text
- Referencing upstream output variables
- Combining custom text with variables

Answer nodes can be added at any stage to stream content to the conversation reply. They support WYSIWYG configuration and rich content mixing, including:
- LLM-generated responses
- Generated images
- Plain text output

---

#### 4. LLM
The **LLM** node invokes large language models to process information provided by the Start node, including natural language input, uploaded files, or images, and generates corresponding responses.

---

#### 5. Knowledge Retrieval
The **Knowledge Retrieval** node retrieves text content relevant to the user’s query from the knowledge base and provides it as contextual input for downstream LLM nodes.

---

#### 6. Question Classifier
The **Question Classifier** node uses LLMs to infer the most appropriate classification for a user query based on predefined category descriptions. The classification results are then passed to downstream nodes to enable more accurate processing.

---

#### 7. IF / ELSE
The **IF / ELSE** node splits the workflow into multiple branches based on conditional expressions such as `if`, `else`, or `elif`.

---

#### 8. Code
The **Code** node supports executing custom **Python** or **NodeJS** scripts within the workflow. It is suitable for tasks such as:
- Arithmetic computation
- JSON transformation
- Text processing

This node greatly enhances developer flexibility by allowing manipulation of variables beyond the capabilities of preset nodes. Users can configure required input and output variables and implement corresponding execution logic.

---

#### 9. Template
The **Template** node enables flexible data transformation and text processing using **Jinja2**, a Python-based templating language.

---

#### 10. Variable Aggregator
The **Variable Aggregator** node aggregates variables from multiple branches into a single unified variable for downstream usage.

This node is particularly useful in multi-branch workflows, allowing variables with equivalent semantics across different branches to be mapped to a single output variable, thereby avoiding redundant variable definitions.

---

#### 11. Parameter Extractor
The **Parameter Extractor** node leverages LLMs to extract structured parameters from natural language input for downstream tool calls or HTTP requests.

It is especially useful when:
- Tools require structured parameter inputs
- Nodes expect specific data formats (e.g., arrays for iteration nodes)

This node converts unstructured user input into machine-readable parameters.

---

#### 12. Iteration
The **Iteration** node repeatedly executes the same operation on each element of an array variable until all elements are processed.

It is commonly used for batch processing tasks.  
For example, in long-text translation, upstream nodes can split a long document into fragments, and iteration nodes can translate each fragment individually to avoid exceeding LLM context limits.

---

#### 13. HTTP Request
The **HTTP Request** node enables sending HTTP requests to external services for tasks such as:
- Fetching external data
- Webhooks
- Image generation
- File downloads

It supports common HTTP methods such as **GET** and **POST**, allowing integration with various external APIs.

---

#### 14. Tools
The **Tools** node provides access to third-party capabilities in three forms:

- **Built-in tools**  
  Dify first-party tools, which may require authorization

- **Custom tools**  
  Tools imported or configured using the OpenAPI / Swagger standard

- **Workflow tools**  
  Published workflows that can be reused as tools in other workflows

---

#### 15. Variable Assigner
The **Variable Assigner** node assigns values to writable variables, including:
- Conversation variables
- Cyclic variables

---

#### 16. Loop
The **Loop** node executes recurring tasks based on the results of previous iterations until:
- Exit conditions are met, or
- A predefined maximum number of loops is reached


