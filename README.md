# Hybrid-RAG-System-for-Historical-Inflation-Data-Analysis
This work presents a comprehensive solution for analyzing historical inflation data.

This project provides a complete, end-to-end solution for exploring historical inflation data and answering user questions using a hybrid Retrieval-Augmented Generation (RAG) system.

It begins by loading and carefully preprocessing an Excel file containing annual inflation values. After ensuring the data is clean and consistent, the system converts the structured dataset into text chunks and embeds them using Sentence-BERT. These embeddings are stored in a FAISS index, enabling fast and efficient retrieval.

What really makes this system stand out is its hybrid design. Instead of relying solely on an LLM, it combines two complementary approaches:

Direct Analytical Functions:
For precise, numerical questions such as
“What’s the highest inflation year?” or “What is the average inflation between 1990 and 2000?”,
the system bypasses the LLM and directly computes the answer using pandas.
This ensures accuracy, determinism, and eliminates the risk of hallucination.

Traditional RAG Pipeline:
For open-ended or context-heavy questions, the system switches to a typical RAG workflow:

Retrieve relevant text chunks from the FAISS index

Use these chunks as context

Let a GPT-2B-it model generate a clear, grounded response

This helps the system handle broader questions where interpretation and explanation are important.

Key Findings

Clean and Complete Data:
The dataset had no missing values in the Annual_Inflation column, making it reliable for analysis.

Inflation Trends:
The highest inflation was recorded in 2022 (286.75) and the lowest in 1914 (10.02).
Certain periods—like 1939–1945, 2000–2010, and 2019–2021—showed consistent upward trends.

Accurate Analytical Responses:
The analytical helper functions worked exactly as intended, giving precise results such as average inflation between 1990–2000 (151.94).

Effective Contextual Reasoning:
When analytical functions didn’t apply, the RAG+LLM pipeline stepped in smoothly.
Importantly, the system avoided hallucinations by recognising when the dataset lacked enough context—such as during questions about “pandemic spikes” not explicitly represented in the data.

Strong Summary Statistics:
Descriptive statistics (mean, quartiles, min, max, etc.) were generated, giving a solid overview of inflation distribution across the years.

Trade-offs in the Design
1. Hybrid RAG (Analytical + LLM)

Benefit:
Combines accuracy (from direct calculations) with flexibility (from the LLM).
Numerical questions are answered perfectly, while interpretive ones still receive rich explanations.

Trade-off:
More complex to build and maintain.
Requires query classification logic and multiple custom functions.

2. Choice of LLM (Gemma-2B-it)

Benefit:
Very lightweight, fast, and ideal for environments like Google Colab or real-time use cases.

Trade-off:
Smaller models may miss deeper reasoning or subtle interpretations that larger LLMs handle better.
Performance strongly depends on how relevant the retrieved context is.

3. Embedding Model (all-MiniLM-L6-v2)

Benefit:
Efficient, fast, and generally produces high-quality embeddings for most tasks.

Trade-off:
May not capture extremely domain-specific details as well as larger or fine-tuned embeddings.

4. FAISS Index (IndexFlatIP)

Benefit:
Very fast similarity search and perfect for cosine-similarity-based embeddings.

Trade-off:
Entire index lives in RAM.
Works great for moderate datasets, but huge datasets would require more advanced FAISS configurations.



