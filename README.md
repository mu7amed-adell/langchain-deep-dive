# LangChain Deep Dive: Text Splitting Techniques

## Overview
This repository provides a comprehensive exploration of text splitting techniques for Retrieval-Augmented Generation (RAG) pipelines. The project demonstrates various LangChain text splitters with practical examples and detailed comparisons.

## Table of Contents
1. [Core Text Splitting Concepts](#core-text-splitting-concepts)
2. [Text Splitter Types](#text-splitter-types)
3. [Practical Examples](#practical-examples)
4. [Choosing the Right Splitter](#choosing-the-right-splitter)
5. [Prerequisites](#prerequisites)
6. [Installation](#installation)
7. [Running the Notebooks](#running-the-notebooks)
8. [Advanced Techniques](#advanced-techniques)
9. [Troubleshooting](#troubleshooting)

## Core Text Splitting Concepts

### Why Split Documents?
Text splitting is essential for several reasons:
- **Handling non-uniform document lengths**: Real-world documents vary in size
- **Overcoming model limitations**: Embedding and language models have maximum input size constraints
- **Improving representation quality**: Smaller chunks lead to more focused and accurate embeddings
- **Enhancing retrieval precision**: Granular splits allow for more precise matching of queries
- **Optimizing computational resources**: Smaller chunks are more memory-efficient

### The Challenge of Chunking
Chunking can split paragraphs or sentences, potentially causing loss of semantic meaning. Proper chunking ensures:
- **Semantic Integrity**: Each chunk retains enough context to be meaningful
- **Retrieval Quality**: Affects how well your RAG system finds relevant information
- **Model Performance**: Impacts how effectively LLMs process and generate responses

## Text Splitter Types

### 1. CharacterTextSplitter
**How it works**:
- Splits text at specific character-level separators (default: `\n\n`)
- Rigidly follows the defined separator without considering semantic boundaries

**When to use**:
- Simple, predictable splits based on a specific delimiter
- Processing structured text with clear, consistent separators
- Working with code or other non-natural language text

**Example**:
```python
from langchain.text_splitter import CharacterTextSplitter
splitter = CharacterTextSplitter(chunk_size=100, chunk_overlap=20, separator=' ')
```

### 2. RecursiveCharacterTextSplitter
**How it works**:
- Uses a hierarchy of separators (`["\n\n", "\n", " ", ""]`)
- Attempts to split at larger semantic units first (paragraphs → sentences → words)
- Preserves more context than simple character splitting

**When to use**:
- Natural language text where preserving semantic meaning is crucial
- Documents of varying structures and lengths
- General-purpose splitting for most RAG applications

**Example**:
```python
from langchain.text_splitter import RecursiveCharacterTextSplitter
splitter = RecursiveCharacterTextSplitter(chunk_size=100, chunk_overlap=20)
```

### 3. TokenTextSplitter
**How it works**:
- Splits text based on token counts rather than characters
- Uses model-specific tokenizers (default: GPT-2)
- Ensures chunks fit within model's token limits

**When to use**:
- Preparing text for specific LLMs with strict token windows
- Precise control over token counts in each chunk
- Working with models that have special token requirements

**Example**:
```python
from langchain.text_splitter import TokenTextSplitter
splitter = TokenTextSplitter(chunk_size=100, chunk_overlap=20)
```

### 4. MarkdownHeaderTextSplitter
**How it works**:
- Splits markdown documents at specified header levels
- Preserves header hierarchy in chunk metadata
- Maintains relationship between headers and their content

**When to use**:
- Processing markdown documentation or technical content
- When header structure provides important contextual information
- Creating chunks that maintain document organization

**Example**:
```python
from langchain.text_splitter import MarkdownHeaderTextSplitter
headers_to_split_on = [("#", "Header 1"), ("##", "Header 2")]
splitter = MarkdownHeaderTextSplitter(headers_to_split_on=headers_to_split_on)
```

## Practical Examples

The `deep_dive_splitting.ipynb` notebook contains comprehensive examples demonstrating:

1. **Basic Splitting Comparison**: Shows how different splitters handle the same text
2. **Real Document Processing**: Applies splitters to PDF documents
3. **RAG Integration**: Demonstrates how split chunks perform in retrieval scenarios
4. **Token Analysis**: Compares character-based vs token-based splitting

**Key Insights from Examples**:
- Recursive splitting preserves semantic meaning better than character splitting
- Token splitting ensures chunks stay within model limits but may break semantic boundaries
- Markdown splitting maintains document structure for better contextual retrieval

## Choosing the Right Splitter

| Splitter Type          | Best For                          | Not Ideal For               |
|------------------------|-----------------------------------|-----------------------------|
| CharacterTextSplitter  | Simple, predictable splits        | Semantic preservation       |
| RecursiveCharacter     | General NLP, varied documents     | Precise split control       |
| TokenTextSplitter      | Model-specific token limits       | Human-readable organization |
| MarkdownHeader         | Structured markdown content       | Plain text processing       |

**Key Considerations**:
1. **Content Type**: Match the splitter to your document format
2. **Use Case**: Consider semantic preservation vs exact token counts
3. **Downstream Processing**: Ensure splits align with embedding model and LLM requirements
4. **Performance**: Recursive splitting is more computationally intensive

## Prerequisites
- **Ollama**: [Installation guide](https://ollama.com/)
  ```bash
  ollama pull llama3.2
  ollama pull nomic-embed-text
  ```
- **Python**: 3.10+ with virtual environment
- **Jupyter Notebook**: For interactive exploration

## Installation
1. Clone repository:
   ```bash
   git clone <repository-url>
   cd langchain-deep-dive
   ```
2. Set up virtual environment:
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # Windows: .venv\Scripts\activate
   ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

## Running the Notebooks

### Deep Dive Splitting Notebook (`deep_dive_splitting.ipynb`)
This notebook provides comprehensive examples of:
- Text splitting techniques comparison
- Real-world document processing
- RAG pipeline integration
- Performance analysis

Start Jupyter Notebook:
```bash
jupyter notebook
```

## Advanced Techniques

### Text Splitting Strategies
- **Character-based**: Fixed-size chunks
- **Token-aware**: Respects token boundaries
- **Semantic**: Natural language boundaries
- **Recursive**: Hierarchical splitting
- **Overlap Strategies**: Context-preserving overlap between chunks

### Retrieval Optimization
- Query expansion techniques
- Reranking strategies for better relevance
- Dynamic chunk sizing based on content type

## Troubleshooting

### Common Issues
- **Ollama connection errors**:
  ```bash
  ollama serve
  ```
- **Missing embeddings**:
  ```bash
  ollama pull nomic-embed-text
  ```
- **Python dependency conflicts**:
  ```bash
  pip install --force-reinstall -r requirements.txt
  ```

### Splitter-Specific Issues
- **Character splitter not splitting**: Check if the separator exists in your text
- **Recursive splitter no overlap**: Overlap only appears when semantic pieces are split into sub-chunks
- **Token splitter breaking words**: This is normal behavior for token-based splitting

## Future Directions

### Planned Enhancements
- Multi-lingual text splitting support
- Advanced semantic splitting algorithms
- Automated splitter selection based on content analysis
- Integration with more embedding models and vector stores

### Research Areas
- Optimal chunk size determination
- Cross-lingual retrieval optimization
- Explainable AI for split quality assessment

## Contributing
We welcome contributions! Please see the notebook examples and add your own splitting techniques or improvements.

## License
This project is licensed under the MIT License.
