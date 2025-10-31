Gen-Ion Discovery Engine

Gen-Ion is an AI-powered discovery engine for designing and validating novel, next-generation battery materials.

This project tackles the single biggest bottleneck in the green energy transition—our reliance on rare, expensive, and ethically-problematic materials like lithium and cobalt. By focusing on abundant and cheap materials like Sodium (Na), Gen-Ion uses generative AI to invent and pre-validate new solid-state electrolytes, accelerating R&D from decades to days.

This repository contains two parts:

A "Rock Solid" Frontend: A sleek, interactive web demo (index.html) that simulates the AI pipeline and showcases the final results with an interactive 3D crystal viewer.

A "Real" Backend Pipeline: A set of Python scripts that use real, pre-trained research models (CDVAE and ALIGNN) to generate and validate new materials.

🚀 The "Wow" Factor: Frontend Demo

The frontend is a standalone index.html file that provides a "rock solid" user experience for a materials scientist. It simulates the backend AI process and presents the final, AI-generated candidates in an interactive 3D crystal viewer built with Three.js.

✨ Key Features

Generative AI Core (CDVAE): Uses a pre-trained Crystal Diffusion Variational Autoencoder to invent thousands of new, chemically-plausible crystal structures from scratch.

Predictive AI Validator (ALIGNN): Employs a Graph Neural Network (GNN) to predict the stability (formation energy) of the newly-generated materials, filtering the "needle from the haystack."

Interactive 3D Viewer: A polished frontend built with Tailwind CSS and Three.js to visualize and interact with the 3D atomic structures of the AI's top candidates.

Real-World Data: The backend pipeline is built to run on real data, using pymatgen to fetch known stable compounds from the Materials Project database.

🔬 How It Works: The AI Pipeline

The project's backend uses a two-step AI process to discover new materials:

The Generator (CDVAE): The 02_generate_candidates.py script loads a pre-trained CDVAE model. This model has learned the "rules of physics and chemistry" from the Materials Project database. We "prompt" it with a set of elements (e.g., Na, Si, O) and it generates 100s of new, hypothetical .cif (Crystallographic Information File) structures.

The Validator (ALIGNN): The 03_validate_candidates.py script takes these 100s of new structures and feeds them one-by-one into a pre-trained GNN (ALIGNN). This "validator" model acts like a virtual lab, predicting the material's formation energy—a key metric for stability.

The Result: The pipeline outputs a simple .csv file, validation_results.csv, ranking all the AI-invented materials from most stable (most promising) to least.

🛠️ Technology Stack

Area

Technologies

Frontend

HTML5, Tailwind CSS, Three.js (for 3D visualization)

Backend

Python, PyTorch (for AI models)

Materials Science

Pymatgen, Materials Project API

AI Models

CDVAE (The "Generator"), ALIGNN (The "Validator" GNN)

Data

Pandas, JSON, .cif (Crystallographic Information Files)

📦 Getting Started & Installation

You can run the Frontend simulation and the Backend pipeline separately.

1. Frontend (The Web Demo)

This part has zero setup.

Make sure you have all the files from this repository.

Simply open index.html in any modern web browser (like Chrome or Firefox).

Click "Start Generation Job" to see the simulated pipeline and results.

2. Backend (The Real AI Pipeline)

This requires a Python environment and downloading the pre-trained models.

Create a Virtual Environment:

# Create a new folder for your backend
mkdir gen-ion-backend
cd gen-ion-backend

# Create a Python virtual environment
python3 -m venv venv

# Activate it
# On macOS/Linux:
source venv/bin/activate
# On Windows:
.\venv\Scripts\activate


Install PyTorch:
Install PyTorch first, following the instructions on the Official PyTorch Website.
Example (for a modern NVIDIA GPU):

pip3 install torch torchvision torchaudio --index-url [https://download.pytorch.org/whl/cu118](https://download.pytorch.org/whl/cu118)


Install All Other Libraries:
Once PyTorch is installed, use the requirements.txt file to install all other dependencies.

pip install -r requirements.txt


Usage: Running the Backend Pipeline

Follow these steps in your terminal (with your venv activated).

Step 1: Get Your Materials Project API Key

Go to https://materialsproject.org/.

Create a free account and log in.

Go to your Dashboard/API page and copy your API Key.

Open 01_fetch_data.py and paste your API key into the API_KEY variable.

Step 2: Run 01_fetch_data.py (Optional)

This script downloads the training data you would use to train your own model.

python 01_fetch_data.py


This creates a training_data/ folder filled with .cif files.

Step 3: Run 02_generate_candidates.py (The Generator)

This script uses a pre-trained CDVAE model to generate 20 new materials.

python 02_generate_candidates.py


This will automatically download the pre-trained model (~85MB) and create a generated_candidates/ folder with 20 new .cif files.

Step 4: Run 03_validate_candidates.py (The Validator)

This script uses a pre-trained ALIGNN model to predict the stability of your new materials.

python 03_validate_candidates.py


This will read the files from generated_candidates/ and create the final report: validation_results.csv.

After this step, you can open validation_results.csv in Excel to see your AI-generated materials, ranked by stability!

📂 Project Structure

.
├── 01_fetch_data.py         # Script to download training data from Materials Project
├── 02_generate_candidates.py  # Script to GENERATE new materials (using pre-trained CDVAE)
├── 03_validate_candidates.py  # Script to VALIDATE new materials (using pre-trained ALIGNN)
├── index.html               # The "rock solid" frontend demo and 3D viewer
├── INSTRUCTIONS.md          # Detailed setup guide for the backend
├── README.md                # This file
├── requirements.txt         # Python libraries for the backend
│
├── training_data/           # (Created by 01_fetch_data.py)
│   └── mp-123_NaO2.cif
│
├── generated_candidates/    # (Created by 02_generate_candidates.py)
│   └── gen_001_Na2SiO3.cif
│
└── validation_results.csv   # (Created by 03_validate_candidates.py) - THE FINAL RESULT


🙏 Acknowledgements

This project is a high-level application built on the incredible work of many researchers. It would not be possible without their open-source models and data.

CDVAE: Tian, X., Xu, J., Wang, Y. et al. "Crystal diffusion variational autoencoder for periodic material generation." npj Comput Mater 9, 73 (2023).

ALIGNN: Chowdhury, N. et al. "Atomistic Line Graph Neural Network for improved materials property predictions." npj Comput Mater 8, 126 (2022).

Materials Project: The Materials Project for providing the foundational data that makes this research possible.

Pymatgen: The Pymatgen library for materials analysis.
