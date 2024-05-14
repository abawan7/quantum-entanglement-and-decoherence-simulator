![PyPI - Python Version](https://img.shields.io/pypi/pyversions/qiskit)
# Quantum Entanglement and Decoherence Simulator

## Table of Contents
1. [Project Overview](#project-overview)
2. [Project Description](#project-description)
3. [Installation Instructions](#installation-instructions)
    - [Prerequisites](#prerequisites)
    - [Install Qiskit](#install-qiskit)
    - [Configure IBM Quantum Account](#configure-ibm-quantum-account)
4. [Running the Program](#running-the-program)
    - [Cell 1: IBM Quantum Account Configuration](#cell-1-ibm-quantum-account-configuration)
    - [Cell 2: Quantum Circuit Setup](#cell-2-quantum-circuit-setup)
    - [Cell 3: Noise Model Implementation](#cell-3-noise-model-implementation)
    - [Cell 4: Execution and Results Collection](#cell-4-execution-and-results-collection)
    - [Cell 5: Results Visualization](#cell-5-results-visualization)
5. [Conclusion and Expected Outcomes](#conclusion-and-expected-outcomes)
6. [Credits](#credits)


## Project Overview
This project aims to develop and simulate a quantum circuit that models entanglement between four qubits while introducing controlled noise to study the effects of decoherence. The goal is to evaluate how noise impacts quantum entanglement across various quantum computing backends and to visualize the changes using different methods.

## Project Description
The Quantum Entanglement and Decoherence Simulator is designed to explore quantum computing principles such as superposition, entanglement, and decoherence in a controlled setting. By simulating a 4-qubit quantum circuit with specific quantum gates and a noise model, this project provides insights into the practical challenges of quantum computing, including error rates and noise effects.

### Quantum Circuit Features
- **Qubits:** Utilizes a 4-qubit register.
- **Gates:** 
  - **Hadamard Gate (H):** Creates a superposition state across all qubits.
  - **Controlled-Z (CZ) and CNOT Gates:** Facilitate entanglement between qubits.
  - **T-Gate and S-Gate:** Introduce phase shifts to increase state complexity.
  - **Pauli-Z Gate:** Implemented to demonstrate phase flip effects on a qubit.
- **Noise Model:** Employs depolarizing noise to simulate environmental interference, introducing an error probability of 0.02 per gate operation.

### Computational Backends
1. **Aer Simulator:** Tests the circuit in an ideal, noise-free environment to establish baseline behaviors.
2. **IBM Quantum Computer (e.g., ibmq_manila):** Provides a real-world context where actual device characteristics and noise influence the outcomes.

### Visualization Techniques
- **State Vector Visualization:** Allows observing the probability amplitudes for each state of the system, useful in analyzing pre and post-noise effects.
- **Quantum Sphere Visualization:** Offers a geometric representation of qubit states, helping visualize the impact of gates and noise.

## Installation Instructions

### Prerequisites
Before you start, ensure that you have Anaconda installed on your system. Anaconda simplifies package management and deployment and helps manage different project environments. If you do not have Anaconda installed, follow the steps below to download and install it.

#### Installing Anaconda
1. **Download Anaconda Installer:**
   - Go to the [Anaconda Distribution page](https://www.anaconda.com/products/individual).
   - Download the appropriate installer for your operating system (Windows, macOS, or Linux).

2. **Install Anaconda:**
   - **Windows:** Open the downloaded `.exe` file and follow the on-screen instructions.
   - **macOS:** Open the downloaded `.pkg` file and follow the on-screen instructions.
   - **Linux:** Open a terminal and run the following command:
     ```bash
     bash ~/Downloads/Anaconda3-2022.05-Linux-x86_64.sh  # Adjust the filename as necessary
     ```
   - Follow the on-screen prompts to complete the installation. Make sure to agree to add Anaconda to your PATH environment variable if you desire.

### Create a New Virtual Environment
Creating a virtual environment is a crucial step as it helps in managing dependencies and keeps your project isolated from other Python projects.

1. **Open your terminal or Anaconda Prompt.**

2. **Create a new virtual environment:**
   ```bash
   conda create --name quantum_env python=3.8
3. **Activate the newly created environment:**
  ```bash
  conda activate quantum_env
  ```

### Install Qiskit
Run the following command in your terminal to install Qiskit and its dependencies:
**Qiskit:** The main library that includes the core quantum circuit functionality.
```bash
pip install qiskit
```
**qiskit-ibmq-provider:** This package allows you to connect to IBM Quantum computers and simulators.

```bash
pip install qiskit-ibmq-provider qiskit-aer
```

**qiskit-aer**: The package that provides high-performance simulators with noise modeling capabilities.

```bash
pip install qiskit-aer
```

### Configure IBM Quantum Account

Replace the placeholder in the provided code with your actual IBM Quantum token. If you do not have an account, you will need to create one and generate a token from the IBM Quantum website.

### Running the Program
The program is structured in a way that it can be run as a script or interactively in a Jupyter Notebook. Below are the detailed steps and descriptions of each code section:

### Cell 1: IBM Quantum Account Configuration

**Purpose:** Ensures that the IBM Quantum account is active; loads the account using your provided token. also Ensure that all the qiskit library that are need is installed

**Key Commands:** IBMQ.load_account()

```python
from qiskit import QuantumCircuit, Aer, IBMQ, execute
from qiskit.visualization import plot_histogram, plot_bloch_multivector
from qiskit.providers.aer.noise import NoiseModel, depolarizing_error # type: ignore

# Check if IBMQ account is already loaded, otherwise, load it with the token
if IBMQ.active_account() is None:
    IBMQ.save_account('03927d9258ef7fd9fbacf9dbee829b761fbf0f8beea3966880e3a8870bfec3badb1ff3d4425e42361746c5ed0ca9e207cb34890dcddbf7929846390b60ba5c45', overwrite=True)
    IBMQ.load_account()
else:
    print("IBM Quantum account already loaded.")

```


### Cell 2: Quantum Circuit Setup

**Purpose:** Initializes the quantum circuit with 4 qubits and applies various gates to generate entanglement and introduce phases.

**Key Commands:** QuantumCircuit, h, cz, cx, t, s, z, measure

```python
qc = QuantumCircuit(4, 4)
qc.h(range(4))  # Apply Hadamard to all qubits
qc.cz(0, 1)  # Controlled-Z gate
qc.cx(1, 2)  # CNOT gate
qc.t(3)  # T gate on qubit 3
qc.s(2)  # S gate on qubit 2
qc.z(1)  # Pauli-Z on qubit 1
qc.measure(range(4), range(4))
print(qc)
```


### Cell 3: Noise Model Implementation
**Purpose:** Configures the noise model to simulate realistic environmental interference on quantum operations.

**Key Commands:** NoiseModel, depolarizing_error, add_all_qubit_quantum_error

```python
noise_model = NoiseModel()
error = depolarizing_error(0.02, 1)
noise_model.add_all_qubit_quantum_error(error, ['h', 'cx', 'cz', 't', 's', 'z'])
```

### Cell 4: Execution and Results Collection
**Purpose:** Executes the quantum circuit on both a simulated backend with noise modeling and a real quantum device to compare outcomes.

**Key Commands:** execute, result, get_counts.

```python
simulator = Aer.get_backend('qasm_simulator')
result = execute(qc, simulator, noise_model=noise_model).result()
counts = result.get_counts(qc)
plot_histogram(counts)
```


### Cell 5: Results Visualization
**Purpose:** Visualizes the outcomes using histograms and state vector plots to analyze and compare the effects of simulation versus real execution.

**Key Commands:** 'plot_histogram', 'plot_bloch_multivector'
```pyhton
provider = IBMQ.get_provider(hub='ibm-q')
real_device = provider.get_backend('ibmq_manila')
job = execute(qc, backend=real_device)
real_counts = job.result().get_counts()
plot_histogram([counts, real_counts], legend=['Simulated', 'Real'])
```

### Conclusion and Expected Outcomes
The Quantum Entanglement and Decoherence Simulator provides a unique opportunity to explore and understand the fundamental aspects of quantum mechanics through practical simulation. By examining the impact of various noise models on a 4-qubit system, this project aims to elucidate how environmental factors affect quantum entanglement and quantum computing operations in general. The outcomes expected from this study include:

1. **Detailed Insights:** Gaining a deeper understanding of the quantum decoherence process and the dynamics of entangled states under the influence of external noise.
2. **Comparative Analysis:** By running the simulations on both ideal and real quantum backends, we can compare theoretical predictions with actual results, highlighting the discrepancies caused by practical limitations.
3. **Noise Mitigation Strategies:** The project will help in identifying effective strategies to mitigate noise in quantum circuits, which is crucial for the development of more robust quantum computing systems.
4. **Educational Value:** Providing a hands-on experience with quantum circuits for educational purposes, helping students and enthusiasts understand complex quantum phenomena in a more intuitive and practical manner.

This project not only contributes to the theoretical studies of quantum mechanics but also has practical implications for the future development of quantum technologies.

### Credits
This project was made possible by the hard work and dedication of the following individuals:

- **Abdullah Awan (21L-7713)**: mlkabawan336@gmail.com

We express our gratitude for their contributions and commitment to advancing the field of quantum computing.
