# TaskVector-In-Context-Learning-Evaluator
In-Context Learning Task Vectors Experiments

Table of Contents
Overview
Background
Experiment Scenarios
Zero-Shot with Instruction
Zero-Shot without Instruction
Few-Shot
Modified Embeddings (Replace/Add)
Repository Structure
Setup
Running the Experiments
Results
Contributing
Citations
License
Overview
This repository implements experiments inspired by the paper "In-Context Learning Creates Task Vectors" by Roee Hendel, Mor Geva, and Amir Globerson. The primary objective is to explore and validate the hypothesis that In-Context Learning (ICL) in Large Language Models (LLMs) operates by compressing a set of demonstrations into a single task vector, which then modulates the model's behavior to perform specific tasks.

By conducting a series of experiments, this project aims to understand the underlying mechanisms of ICL and assess the stability and robustness of task vectors across different scenarios and layers within the OPT-IML 1.3B model.

Background
In-Context Learning (ICL) has emerged as a powerful paradigm where LLMs can perform new tasks based on a few demonstrations provided in the input prompt. Despite its empirical success, the underlying mechanisms of ICL remain not fully understood. The paper by Hendel et al. proposes that ICL works by mapping the set of demonstrations into a task vector (θ), which serves as a parameter for a hypothesis class function, effectively enabling the model to perform the desired task.

This repository replicates and extends the experiments presented in the paper, providing a framework to analyze how task vectors influence model performance across different scenarios and layers.

Experiment Scenarios
The experiments are designed to evaluate the performance and robustness of task vectors under various prompting strategies and embedding modifications. The following scenarios are explored:

Zero-Shot with Instruction
Prompt Structure:
mathematica
Copy code
Classify the sentiment of the following sentences as Positive or Negative.
[Test Sentence] :
Description: The model is provided with an instructional prompt without any training examples. It relies solely on the instruction and the test sentence to make predictions.
Zero-Shot without Instruction
Prompt Structure:
csharp
Copy code
[Test Sentence] :
Description: The model receives only the test sentence followed by a colon, without any instructional context or training examples.
Few-Shot
Prompt Structure:
mathematica
Copy code
Classify the sentiment of the following sentences as Positive or Negative.
[Train Sentence 1] [Positive/Negative]
[Train Sentence 2] [Positive/Negative]
...
[Test Sentence] :
Description: A balanced set of training examples (few-shot) is included in the prompt to guide the model's predictions. The number of shots (e.g., 2, 4, 16) can be varied to assess their effect on performance.
Modified Embeddings (Replace/Add)
Process:
Theta Extraction: A separate training sample is used to extract the embedding (θ) of the colon : token from a specified layer.
Embedding Modification: The extracted θ is either replaced or added to the embedding of the colon token in a target layer.
Prediction: The modified model is used to make predictions on the test set using a zero-shot prompt without instruction.
Description: This scenario investigates how altering specific layer embeddings affects the model's sentiment classification performance.
Repository Structure
arduino
Copy code
In-Context-Learning-Task-Vectors/
├── data/
│   └── README.md
├── experiments/
│   ├── __init__.py
│   ├── config.py
│   ├── dataset_handler.py
│   ├── model_handler.py
│   ├── prompt_constructor.py
│   ├── experiment_runner.py
│   └── main.py
├── results/
│   └── README.md
├── scripts/
│   └── setup.sh
├── tests/
│   └── test_experiment.py
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
data/: Contains datasets or data-related scripts.
experiments/: Main module with experiment configurations, handlers, and runner.
results/: Stores output results such as JSON summaries and plots.
scripts/: Utility scripts for setup or other tasks.
tests/: Contains test cases to ensure code reliability.
.gitignore: Specifies files and directories to ignore in Git.
LICENSE: Project licensing information.
README.md: Project overview and documentation.
requirements.txt: Lists all Python dependencies.
Setup
Prerequisites
Python 3.8 or higher
PyTorch (compatible with your CUDA version if using GPU)
Transformers library by Hugging Face
Datasets library by Hugging Face
Other dependencies listed in requirements.txt
Installation
Clone the Repository

bash
Copy code
git clone https://github.com/yourusername/In-Context-Learning-Task-Vectors.git
cd In-Context-Learning-Task-Vectors
Create a Virtual Environment

bash
Copy code
python3 -m venv venv
source venv/bin/activate
Install Dependencies

bash
Copy code
pip install --upgrade pip
pip install -r requirements.txt
(Optional) Run Setup Script

If you prefer using the setup script:

bash
Copy code
chmod +x scripts/setup.sh
./scripts/setup.sh
Running the Experiments
Navigate to the experiments directory and execute the main script:

bash
Copy code
cd experiments
python main.py
Configuration
Modify the config.py file to adjust experiment parameters such as:

model_name: Pretrained model to use (default: "facebook/opt-iml-1.3b").
seed: Random seed for reproducibility (default: 313).
test_samples: Number of test samples (default: 1000).
device: Compute device ("cuda" or "cpu").
batch_size: Batch size for processing (default: 32).
output_dir: Directory to save results (default: "experiment_results_SST-2_final").
shot_sizes: List of shot sizes for few-shot experiments (default: [2, 4, 16]).
num_runs: Number of runs to perform for each shot size (default: 3).
layer_pairs: Layers for theta extraction and injection (default: [(8, 9), (13, 14), (19, 20)]).
Results
After running the experiments, results are saved in the results/ directory, including:

multi_run_results.json: JSON file containing mean and standard deviation of accuracies across runs.
accuracy_comparison_multi_run.png: Plot comparing accuracies across different methods and shot sizes.
Interpretation of Results
The experiments evaluate how different prompting strategies and embedding modifications impact the model's performance on sentiment analysis tasks. By conducting multiple runs with varying few-shot examples and theta vectors, the study assesses the stability and robustness of each scenario.

Zero-Shot with Instruction and Zero-Shot without Instruction scenarios provide baseline performance without leveraging training examples.
Few-Shot experiments demonstrate the effect of providing a small, balanced set of training examples.
Modified Embeddings (Replace/Add) scenarios explore how injecting task vectors at specific layers influences the model's predictions.
The aggregated results (mean ± std) offer insights into the consistency and reliability of each approach.

Contributing
Contributions are welcome! Please follow these steps:

Fork the Repository

Create a Feature Branch

bash
Copy code
git checkout -b feature/YourFeature
Commit Your Changes

bash
Copy code
git commit -m "Add Your Feature"
Push to the Branch

bash
Copy code
git push origin feature/YourFeature
Open a Pull Request

Please ensure your contributions adhere to the project's coding standards and include appropriate tests.

Citations
If you find this repository useful for your research or projects, please cite the following paper:

bibtex
Copy code
@article{hendel2023incontext,
  title={In-Context Learning Creates Task Vectors},
  author={Hendel, Roee and Geva, Mor and Globerson, Amir},
  journal={arXiv preprint arXiv:2310.15916},
  year={2023}
}
License
This project is licensed under the MIT License.

Additional Recommendations
Include the Paper in the Repository:

Add a papers/ directory containing the PDF of "In-Context Learning Creates Task Vectors" and any related works.
Add Badges:

Enhance visibility with additional badges (e.g., build status, code coverage).
markdown
Copy code
![Build Status](https://img.shields.io/github/actions/workflow/status/yourusername/In-Context-Learning-Task-Vectors/python-app.yml?branch=main)
![Coverage](https://img.shields.io/codecov/c/github/yourusername/In-Context-Learning-Task-Vectors)
Detailed Documentation:

Create a /docs folder for extended documentation, including detailed methodology, experiment setups, and theoretical background.
Examples and Tutorials:

Provide example scripts or Jupyter notebooks demonstrating how to use the code for specific tasks.
Continuous Integration (CI):

Set up GitHub Actions or another CI tool to automate testing and ensure code quality.
Issue and Pull Request Templates:

Facilitate standardized contributions by adding templates in .github/ISSUE_TEMPLATE/ and .github/PULL_REQUEST_TEMPLATE.md.
Add a CONTRIBUTING.md:

Provide guidelines for contributing to the project, outlining code standards, branching strategies, and testing requirements.
Include a CHANGELOG.md:

Maintain a changelog to document significant changes, updates, and version history.
