 # Project- hybrid OA-RAG #

**Introduction**

     Retrieval-augmented generation (RAG) models have demonstrated significant potential in natural language processing tasks for specialized retrieval. However, current retrieval augmented generation models often suffer from inefficient and imprecise retrieval in specified domains, such as the semiconductor industry, leading to sub-optimal performance. This study proposes the incorporation of domain-specific ontology to address these limitations. By leveraging ontological structures and semantic relationships, the proposed approach facilitates the more effective retrieval of relevant information from knowledge bases. In addition, the use of ontology-aware word embeddings, such as owl2vec and open API embeddings, further improves the precision of query-document matching. The proposed ontology-augmented RAG model was evaluated in the semiconductor research domain, demonstrating its potential for enhancing idea generation, providing research assistance, and facilitating innovation in the scientific and technological fields. The results showed improved retrieval accuracy, context awareness, and knowledge integration, ultimately enabling more efficient and precise data retrieval for the RAG model. 

Problem: LLMs struggle with hallucinations and lack of interpretability, hindering their reliability.
Existing Solutions: Existing RAG models face limitations in retrieval, computational cost, bias, and domain expertise.
Hybrid: We introduce Ontology-Augmented RAG (OA-RAG) to improve RAG models by integrating domain ontologies and research data for more accurate, explainable, and domain-specific outputs.
Impact: OA-RAG enables efficient retrieval and researchers to generate ideas, answer questions, and conduct a literature review

**Implimentaiom:**

OA-RAG Implementation: The hybrid system, called OA-RAG (Ontology-Augmented RAG), processes user questions through NLP, converts the ontology into a Neo4j knowledge graph, based on the semiconductor  concepts searches the graph and relevant research papers using the arXiv API, and combines the information to generate comprehensive ideas and answers using a powerful language model GPT 3.5 turbo which promted for idea genaration.

**Evaluation:** Tested OA-RAG using the benchmark dataset to assess responses, accuracy, relevance, and clarity.

**Analysis:** Compared OA-RAG to existing solutions (Naive RAG, Advanced RAG, Gragh RAG), highlighting the benefits of using domain-specific ontologies for semiconductor.


**Step-1 User query preprocessing-(NLTK tool)**

Stop words removal, Stemming Lemmatization, Parts of speech tagging, Named entity recognisation, Handleing variation.

**Step-2 Crafting the datasets**

Ontology Development: Crafted a knowledge base for semiconductor research using specialized ontologies and concepts linked to research papers from arXiv.


**Step-3 Data retrieval**

BM25 & Sematic retrieval a combined approch for data retrieval

**Step-4 Data ingestion**

Langchain’s utility shines in data ingestion and preparation: ”Loaders” Langchain provides a variety of document loaders that can ingest data from different sources. This could include text files, PDFs, websites, or databases. This system likely helps load data from the ontology CSV file and the arXiv API research papers. Indexes: after loading, Langchain can create efficient indexes of the data. This is crucial for quick retrieval and searching. It might be using techniques like vector indexing to make the semantic and vector searches more efficient. Text to Cypher, suggests Langchain is helping convert natural language queries into Cypher, the query language used by Neo4j. This enables  more intuitive interaction with the graph database. VectorDB and GraphDB, Langchain provides integrations with various vector stores  and graph  databases (Neo4j). It helps in seamlessly connecting these databases to the rest of the system. OpenAI embeddings, Langchain can interface with OpenAI’s API to generate embeddings for text, which are crucial for vector search functionality. OpenAI API keys, Langchain manages API keys securely, allowing the system to interact with OpenAI’s services as needed.


**Step-5 Data genaration**

The GPT-3.5 component in architecture serves as the Generation layer, which is a crucial part of the system’s ability to produce human-like responses. Here’s how it fits into the overall workflow. Input processing, After the user’s query has been preprocessed and relevant information has been retrieved from the various databases (GraphDB, VectorDB, etc.), this contextualized data is passed to the GPT-3.5 model which is promted for ground truth idea genaration . For natural language generation, GPT-3.5 takes the retrieved information and the original query as input. It then generates a natural language response that aims to answer the user’s question or complete their requested task. Context-aware responses Because GPT-3.5 receives input that includes both the user’s query and relevant information from the retrieval layer, it can produce responses that are not just based on its pre-trained knowledge but are  informed by the specific data sources in the system.

**Step-6 Output evaluation**

RAGAs (Retrieval-Augmented Generation Assessments) is a framework that provides metrics and GPT3.5 -generated data to evaluate the performance of Proposed RAG system using metrics that assess both Retrieval and generation outputs. 

Context recall, Context precision,  Answer relevancy, Faithfulness.
 
Dataset Creation: Built a collection of 150 semiconductor-related questions and answers for testing.


**Step-7 Output Visuatlization**

The UI is the crucial interface that bridges the RAG system to the user. It’s how the user interacts with and receives the results of the RAG process.
Displaying Information: The UI determines how the generated answer is presented: Text:A simple text box showing the answer. Visualizations:

Matplotlib tool : Graphs, charts to help visualize the answer. 


**GPT 3.5 is prompted with ground truth idea generation**

     
Prompts are crucial in any language model-based system and require greater consideration than typical strings. A well-crafted prompt should have a clear task instruction written in simple natural language that is accessible to any language model. The goal is to give prompts that are generalizable and do not unnecessarily focus on a single state of the language model. Language models are often thought to be more accurate in few-shot conditions than zero-shot contexts. To make the most of this benefit, every query should be supported with relevant examples.
Prompts in ragas are defined using the Prompt class. Each prompt defined using this class will contain.
name: a name given to the prompt. Used to save and identify the prompt
instruction: The natural language description of the task to be carried out by the LLM
examples: List one or more demonstrations of the task at hand. Using demonstrations converts the task from zero-shot to few-shot which can improve accuracy in most cases.
input_keys: List of one or more variable names that are used to identify the inputs provided to the LLM.
output_key: Variable name that is used to identify the output
output_type: Output type of the prompt. Can be JSON or string.

**Let’s create a prompt**

from ragas.llms.prompt import Prompt

qa_prompt = Prompt(
    name="idea_generation",
    instruction="Generate 3  ideas for the question",
    examples=[
        {
            "Question": "What is the latest chip that NVIDIA has released.",
            "context": "Idea 1: Quantum Dot Transistors
Quantum dot transistors use nanoscale semiconductor particles to improve speed, power efficiency, and miniaturization in electronics.
Idea 2: Two-Dimensional Materials for Transistors
Graphene and other 2D materials offer high performance for transistors due to their exceptional electrical properties.
Idea 3: Organic Transistors for Flexible Electronics
Organic transistors, made from flexible materials, enable bendable and stretchable electronics for wearables and more.
",
            "output": {"Ground truth ideas":generatedd "},
        },
        {
            "answer": "What are the benefits of using 2D materials like graphene in semiconductors? .",
            "context": "Idea 1: Graphene as a tunnel barrier in TFETs improves device performance, enables low-power electronics, and opens the door for advanced transistor designs.
Idea 2: Integrating 2D materials into flexible substrates allows for bendable and stretchable devices, leading to wearable electronics, flexible displays, and conformal sensors.
Idea 3: Stacking different 2D materials creates van der Waals heterostructures with tailored properties, enabling applications in photodetectors, photovoltaic devices, and optoelectronics
.",
            "output": {"Ground truth ideas ":generatedd"?"},
        }
    ],
    input_keys=["answer", "context"],
    output_key="output",
    output_type="json",
)
save(self, cache_dir)
This method will save the prompt to the given cache_dir (default ~/.cache) directory using the value in the name variable.
ground ideas_prompt.save()
The prompts are saved in JSON format to ~/.cache/ragas by default. One can change this by setting the RAGAS_CACHE_HOME environment variable to the desired path. In this example, the prompt will be saved in ~/.cache/ragas/english/question_generation.json
_load(self, language, name, cache_dir)
This method will load the appropriate prompt from the saved directory.
from ragas.utils import RAGAS_CACHE_HOME
Prompt._load(name="groundtruth_idea_generation",language="english",cache_dir=RAGAS_CACHE_HOME)

Prompt(name='Ground truth idea _generation', instruction='Generate a ideas for the given question', examples=[{'answer': "Question": "What is the latest chip that NVIDIA has released.",   "context": "Idea 1: Quantum Dot Transistors
Quantum dot transistors use nanoscale semiconductor particles to improve speed, power efficiency, and miniaturization in electronics.
Idea 2: Two-Dimensional Materials for Transistors,Graphene and other 2D materials offer high performance for transistors due to their exceptional electrical properties.Idea 3: Organic Transistors for Flexible Electronics.Organic transistors, made from flexible materials, enable bendable and stretchable electronics for wearables and more.",   "output": {"Ground truth ideas":generatedd "},
 input_keys=['question', 'context'], output_key='output', output_type='JSON')
