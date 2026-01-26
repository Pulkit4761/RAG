## LangChain package ecosystem
langchain-core        → foundational abstractions
langchain-community  → integrations & loaders
langchain-text-splitters → text chunking logic
langchain            → chains, agents, orchestration


## Document
1) It converts messy real-world data (files, PDFs, web pages) into a standard, structured container that LLMs and embedding models can later consume.

2) from langchain_core.documents import Document

3) It has 2 main parameters-
Document(
    page_content="Some text here",
    metadata={"source": "file.txt"}
)

4) LangChain converts everything into:
Document → chunks → embeddings → retrieval → answer


## langchain_text_splitters
1) Purpose: Contains utilities for splitting large texts into smaller chunks, which is useful for processing, embedding, or searching.

2) Classes:
-> RecursiveCharacterTextSplitter: Splits text recursively by characters, aiming to keep chunks within a certain size.
-> CharacterTextSplitter: Splits text into chunks based on character count.
-> TokenTextSplitter: Splits text based on token count (tokens are often used in NLP models).


## TextLoader
1) Read a .txt file from disk and convert it into a LangChain Document

2) from langchain_community.document_loaders import TextLoader

3) Parameters of TextLoader-
1️⃣ file_path 
TextLoader("notes.txt")

-> Path to the .txt file.

2️⃣ encoding 
TextLoader("notes.txt", encoding="utf-8")

-> Controls how bytes are decoded into text.

3️⃣ autodetect_encoding (optional)
TextLoader("notes.txt", autodetect_encoding=True)

-> Tries multiple encodings automatically
-> Slower
-> Useful for messy datasets

4) Minimal usage 
from langchain_community.document_loaders import TextLoader

loader = TextLoader("data/text_files/uv_package_manager.txt", encoding="utf-8")
documents = loader.load()

## DocumentLoader
1) Loads ALL files from a directory and converts them into LangChain Document objects

2) Basic usage
from langchain_community.document_loaders import DirectoryLoader, TextLoader

loader = DirectoryLoader(
    "data/text_files",
    loader_cls=TextLoader,
    loader_kwargs={"encoding": "utf-8"}
)

documents = loader.load()

3) Important parameters
1️⃣ path
DirectoryLoader("data/text_files")

2️⃣ glob
glob="**/*.txt"
-> Controls which files to load.

Examples:
"*.txt"        # only txt files (current dir)
"**/*.txt"     # txt files + subdirectories
"**/*.md"      # markdown files
"**/*"         # everything

3️⃣ loader_cls
loader_cls=TextLoader
-> Tells LangChain HOW to read each file.

-> Common loaders:
TextLoader → .txt
PyPDFLoader → .pdf
UnstructuredMarkdownLoader → .md
CSVLoader → .csv

4️⃣ loader_kwargs
Pass arguments to the loader itself.

Example:
loader_kwargs={"encoding": "utf-8"}

5️⃣ recursive
recursive=True

True → scan subfolders
False → only top-level files

6️⃣ show_progress
show_progress=True
-> Shows a progress bar when loading many files.

7️⃣ use_multithreading
use_multithreading=True
-> Loads files in parallel
-> Faster for large datasets

8️⃣ silent_errors
silent_errors=True

-> If True:
    Skips unreadable files
    Doesn’t crash the pipeline