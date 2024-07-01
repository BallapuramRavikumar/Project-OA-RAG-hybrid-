 Retrieval-augmented generation (RAG) models have demonstrated significant potential in natural language processing tasks within specialized domains, such as the semiconductor industry. However, current retrieval-augmented generation (RAG) models often suffer from inefficient and imprecise retrieval, leading to sub-optimal performance. This study proposes the incorporation of domain-specific ontology to address these limitations. By leveraging ontological structures and semantic relationships, the proposed approach facilitates a more effective retrieval of relevant information from knowledge bases. In addition, the use of ontology-aware word embedding, such as owl2vec, further improves the precision of query-document matching. The proposed ontology-augmented RAG model was evaluated in the semiconductor research domain, demonstrating its potential for enhancing idea generation, providing research assistance, and facilitating innovation in the scientific and technological fields. The results showed improved retrieval accuracy, context awareness, and knowledge integration, ultimately enabling more efficient and precise data retrieval for the RAG model.# Project-hybrid-RAGGPT 3.5 is prompted with ground truth idea generation:
Prompts are crucial in any language model-based system and require greater consideration than typical strings. A well-crafted prompt should have a clear task instruction written in simple natural language that is accessible to any language model. The goal is to give prompts that are generalizable and do not unnecessarily focus on a single state of the language model. Language models are often thought to be more accurate in few-shot conditions than zero-shot contexts. To make the most of this benefit, every query should be supported with relevant examples.
Prompts in ragas are defined using the Prompt class. Each prompt defined using this class will contain.
name: a name given to the prompt. Used to save and identify the prompt
instruction: The natural language description of the task to be carried out by the LLM
examples: List one or more demonstrations of the task at hand. Using demonstrations converts the task from zero-shot to few-shot which can improve accuracy in most cases.
input_keys: List of one or more variable names that are used to identify the inputs provided to the LLM.
output_key: Variable name that is used to identify the output
output_type: Output type of the prompt. Can be JSON or string.

Let’s create a simple prompt using Prompt


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
