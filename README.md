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
1. **Qiskit Aer Simulator(QASM):** Tests the circuit in an ideal, noise-free environment to establish baseline behaviors.
2. **IBM Quantum Computer (e.g., ibmq_manila):** Provides a real-world context where actual device characteristics and noise influence the outcomes.

### Visualization Techniques
- **State Vector Visualization:** Allows observing the probability amplitudes for each state of the system, useful in analyzing pre and post-noise effects.
- **Quantum Sphere Visualization:** Offers a geometric representation of qubit states, helping visualize the impact of gates and noise.

## Hand-Drawn Circuit Diagram
<img src="https://github.com/abawan7/quantum-entanglement-and-decoherence-simulator/blob/main/Circuit.png" alt="Hand-Drawn Circuit Diagram" width="320"/>

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
pip install qiskit==0.39.0
```
**qiskit-ibmq-provider:** This package allows you to connect to IBM Quantum computers and simulators.

```bash
pip install qiskit-ibmq-provider=0.19.2
```

**qiskit-aer**: The package that provides high-performance simulators with noise modeling capabilities.

```bash
pip install qiskit-aer==0.11.0
```

### Configure IBM Quantum Account

Replace the placeholder in the provided code with your actual IBM Quantum token. If you do not have an account, you will need to create one and generate a token from the IBM Quantum website.

### Running the Program
The program is structured in a way that it can be run as a script or interactively in a Jupyter Notebook. Below are the detailed steps and descriptions of each code section:

### Cell 1: Importing Necessary Libraries
This cell imports all required libraries for creating, simulating, and executing quantum circuits, as well as for visualizing results. It also loads your IBM Quantum API token to access IBM's quantum backends.


**Key Commands:** IBMQ.load_account()

```python
import matplotlib.pyplot as plt
from qiskit import QuantumCircuit, Aer, execute, IBMQ
from qiskit.visualization import plot_histogram, plot_bloch_multivector
from qiskit.providers.aer.noise import NoiseModel, depolarizing_error
import numpy as np

IBMQ.save_account('YOUR_TOKEN')
IBMQ.load_account()

```


### Cell 2: Load IBMQ Account and List Available Backends
This cell retrieves the list of available quantum backends from IBM, which can be used for executing your quantum circuit. It prints out the available backends so you can choose one.

```python
provider = IBMQ.get_provider('ibm-q')
backends = provider.backends()
for backend in backends:
    print(backend)
```


### Cell 3: Define the Quantum Circuit
This cell creates a quantum circuit with 4 qubits and 4 classical bits. Various quantum gates are applied to the qubits, and the circuit is printed and visualized.

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

### Cell 4: Define Noise Model
This cell defines a noise model to simulate realistic conditions where quantum operations are not perfect. It specifies errors for single-qubit and two-qubit gates and adds them to the noise model.

```python
noise_model = NoiseModel()

# Define single-qubit and two-qubit errors
single_qubit_error = depolarizing_error(0.02, 1)
two_qubit_error = depolarizing_error(0.02, 2)

# Add errors to the noise model
noise_model.add_all_qubit_quantum_error(single_qubit_error, ['h', 't', 's', 'z'])
noise_model.add_all_qubit_quantum_error(two_qubit_error, ['cx', 'cz'])
```


### Cell 5: Simulate the Circuit with Noise
This cell simulates the quantum circuit using the `qasm_simulator` backend with the defined noise model. It retrieves the counts of each quantum state after measurement and plots them as a histogram using `matplotlib`.

```python
simulator = Aer.get_backend('qasm_simulator')
result = execute(qc, simulator, noise_model=noise_model).result()
counts = result.get_counts(qc)

ordered_counts = dict(sorted(counts.items()))

labels, values = zip(*ordered_counts.items())

plt.bar(labels, values)

plt.xlabel('Quantum State')
plt.ylabel('Counts')
plt.title('Histogram of Quantum State Counts')

plt.xticks(rotation=90)

plt.show()
```

### Cell 6: Execute the Circuit on the IBMQ Backend with Error Handling
This cell executes the quantum circuit on a real IBMQ backend (`ibm_osaka` in this case) with error handling. It compares the simulated and real counts by plotting them as a histogram using `matplotlib`. The `autolabel` function adds text labels above each bar in the histogram, displaying its height.

```python
# Execute the circuit on the IBMQ backend with error handling
try:
    job = execute(qc, backend=backend)
    real_counts = job.result().get_counts(qc)
except Exception as e:
    print(f"Error executing job: {e}")
    real_counts = {}

# Ensure both counts are ordered
ordered_simulated_counts = dict(sorted(counts.items()))
ordered_real_counts = dict(sorted(real_counts.items()))

# Combine keys from both dictionaries
all_keys = set(ordered_simulated_counts.keys()).union(set(ordered_real_counts.keys()))

# Create lists for plotting
simulated_values = [ordered_simulated_counts.get(key, 0) for key in all_keys]
real_values = [ordered_real_counts.get(key, 0) for key in all_keys]

# Plotting the histogram
import matplotlib.pyplot as plt
import numpy as np

x = np.arange(len(all_keys))  # the label locations
width = 0.35  # the width of the bars

fig, ax = plt.subplots()
rects1 = ax.bar(x - width/2, simulated_values, width, label='Simulated')
rects2 = ax.bar(x + width/2, real_values, width, label='Real')

# Add some text for labels, title, and custom x-axis tick labels, etc.
ax.set_xlabel('Quantum State')
ax.set_ylabel('Counts')
ax.set_title('Simulated vs Real Quantum State Counts')
ax.set_xticks(x)
ax.set_xticklabels(all_keys, rotation=90)
ax.legend()

# Attach a text label above each bar in rects, displaying its height.
def autolabel(rects):
    """Attach a text label above each bar in *rects*, displaying its height."""
    for rect in rects:
        height = rect.get_height()
        ax.annotate('{}'.format(height),
                    xy=(rect.get_x() + rect.get_width() / 2, height),
                    xytext=(0, 3),  # 3 points vertical offset
                    textcoords="offset points",
                    ha='center', va='bottom')

autolabel(rects1)
autolabel(rects2)

fig.tight_layout()

plt.show()
```

### Conclusion and Expected Outcomes
The Quantum Entanglement and Decoherence Simulator provides a unique opportunity to explore and understand the fundamental aspects of quantum mechanics through practical simulation. By examining the impact of various noise models on a 4-qubit system, this project aims to elucidate how environmental factors affect quantum entanglement and quantum computing operations in general. The outcomes expected from this study include:

1. **Detailed Insights:** Gaining a deeper understanding of the quantum decoherence process and the dynamics of entangled states under the influence of external noise.
2. **Comparative Analysis:** By running the simulations on both ideal and real quantum backends, we can compare theoretical predictions with actual results, highlighting the discrepancies caused by practical limitations.
3. **Noise Mitigation Strategies:** The project will help in identifying effective strategies to mitigate noise in quantum circuits, which is crucial for the development of more robust quantum computing systems.
4. **Educational Value:** Providing a hands-on experience with quantum circuits for educational purposes, helping students and enthusiasts understand complex quantum phenomena in a more intuitive and practical manner.

This project not only contributes to the theoretical studies of quantum mechanics but also has practical implications for the future development of quantum technologies.

### Credits

- **Abdullah Awan**: mlkabawan336@gmail.com

We express our gratitude for their contributions and commitment to advancing the field of quantum computing.
