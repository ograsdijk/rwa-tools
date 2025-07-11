# RWA Tools

[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/rwa-tools)](https://pypi.python.org/pypi/rwa-tools/)
[![PyPI - Package Version](https://img.shields.io/pypi/v/rwa-tools)](https://pypi.python.org/pypi/rwa-tools/)

A Python package for working with quantum Hamiltonians in the Rotating Wave Approximation (RWA) frame.

## Overview

RWA Tools provides a symbolic framework for representing, manipulating, and transforming quantum Hamiltonians to simplify their analysis using the rotating wave approximation. The package leverages symbolic mathematics (via SymPy) and graph theory (via NetworkX) to automate the often tedious process of applying the RWA to complex quantum systems.

## Features

- **Symbolic Hamiltonian representation**: Create symbolic Hamiltonians with arbitrary energy levels and couplings
- **Graph-based coupling analysis**: Visualize and analyze quantum state couplings as graph networks
- **Automated RWA transformations**: Transform Hamiltonians into rotating frames and apply the RWA
- **Flexible coupling parameterization**: Control coupling strengths with configurable identifiers (a0, a1, etc.)
- **System decomposition**: Split composite systems into independent subsystems for separate analysis

## Installation

```bash
pip install rwa-tools
```

Or install from source:

```bash
git clone https://github.com/ograsdijk/rwa-tools.git
cd rwa-tools
pip install -e .
```

## Dependencies

- `sympy`: For symbolic mathematics
- `networkx`: For graph representation and analysis
- `numpy`: For numerical operations
- `matplotlib`: For visualization (optional)

## Basic Usage

Here's a simple example demonstrating how to create and transform a three-level quantum system with two driving fields:

```python
import sympy as smp
from rwa_tools import (
    create_hamiltonian_symbolic,
    create_coupling_graph,
    create_hamiltonian_rwa
)
from rwa_tools.graph_transform import create_transform_matrix

# Define a 3-level system with two driving fields
nstates = 3
couplings = [[(0, 1)], [(1, 2)]]  # Two fields: one coupling states 0-1, another coupling 1-2

# Create the symbolic Hamiltonian
hamiltonian = create_hamiltonian_symbolic(couplings, nstates)

# Create the coupling graph
coupling_graph = create_coupling_graph(couplings, nstates)

# Generate the transformation matrix
transform = create_transform_matrix(coupling_graph)

# Apply the RWA transformation
hamiltonian_rwa = create_hamiltonian_rwa(hamiltonian, transform)

# Display the RWA Hamiltonian
print("RWA Hamiltonian:")
smp.pprint(hamiltonian_rwa.hamiltonian)
```

## Advanced Example: Lambda System

Here's an example of analyzing a Lambda-type three-level system:

```python
import matplotlib.pyplot as plt
import networkx as nx
import sympy as smp

from rwa_tools import (
    create_coupling_graph,
    create_hamiltonian_symbolic,
    create_hamiltonian_rwa
)
from rwa_tools.graph_transform import create_transform_matrix

# Lambda system: two ground states (0,1) coupled to one excited state (2)
nstates = 3
couplings = [[(0, 2)], [(1, 2)]]  # Two fields coupling to the excited state

# Create the symbolic Hamiltonian and coupling graph
hamiltonian = create_hamiltonian_symbolic(couplings, nstates)
coupling_graph = create_coupling_graph(couplings, nstates)

# Visualize the coupling graph
plt.figure(figsize=(8, 6))
pos = nx.spring_layout(coupling_graph)
nx.draw(coupling_graph, pos, with_labels=True, node_size=800, node_color='lightblue',
        font_size=12, font_weight='bold')
plt.title("Lambda System Coupling Graph")
plt.show()

# Generate the transformation matrix and apply RWA
transform = create_transform_matrix(coupling_graph)
hamiltonian_rwa = create_hamiltonian_rwa(hamiltonian, transform)

# Show the RWA Hamiltonian
print("RWA Hamiltonian for Lambda System:")
smp.pprint(hamiltonian_rwa.hamiltonian)
```

## Coupling Identifiers

For systems with multiple couplings sharing the same frequency, RWA Tools uses a coefficient system with identifiers in the format `a{number}` (a0, a1, etc.):

```python
# A system where multiple transitions share the same driving frequency
nstates = 5
couplings = [[(0, 4), (1, 4), (2, 4)]]  # Three states coupled to state 4 by the same field

hamiltonian = create_hamiltonian_symbolic(couplings, nstates)
coupling_graph = create_coupling_graph(couplings, nstates)

# The coupling terms will use identifiers a0, a1, a2 to allow independent control
# of the coupling strengths while sharing the same frequency
print("Coupling Matrix with Identifiers:")
smp.pprint(hamiltonian.coupling_matrix)
```

## License

MIT License