# MP1 – Prompt Lab

Compare four LLM prompting strategies on the same task and evaluate their performance.

## What This Project Does

* Loads a dataset of **10 text snippets**.
* Applies **4 different prompt strategies** to each snippet.
* Runs **40 LLM calls asynchronously**.
* Captures response, extraction, cost, and latency.
* Evaluates each response using:

  * **Accuracy** – 0 to 3 fields matched
  * **Parse Success** – successful/unsuccessful extraction
  * **LLM Judge Score** – 1 to 4
* Builds a comparison table to analyze the performance of each strategy.

## Project Structure

```text
.
|-- README.md
|-- MP1_Prompt_Lab.ipynb
|-- mp1_writeup.md
|--requirements
|--docs-adr-0002-prompting-strategy.md
```

## How to Run

1. Install the required Python dependencies.
2. Configure the required LLM/API credentials.
3. Open `MP1_Prompt_Lab.ipynb` in Jupyter Notebook or JupyterLab.
4. Run the notebook cells sequentially.

## Evaluation

The final comparison table helps identify which prompting strategy provides the best balance of **accuracy, parsing reliability, latency, and cost**.

## Goal

The goal of this project is to understand how different prompting strategies affect **LLM output quality and performance**.
