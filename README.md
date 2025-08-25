# 🔷 Honeycomb Random Walk Simulator

A Python implementation of random walks on hexagonal (honeycomb) lattices with color-based navigation rules and customizable boundary conditions.

## 📋 Overview

This repository contains two interconnected components for simulating and analyzing random walks on honeycomb lattices:

1. **Honeycomb Lattice Generator** - Creates customizable hexagonal grids with probabilistic coloring
2. **Honeycomb Walk Simulator** - Performs random walks on the lattice edges with color-based turning rules

The walker navigates by looking at adjacent hexagons and turning based on their colors:
- **Gray hexagon** → Turn 60° right
- **White hexagon** → Turn 60° left

Special boundary conditions create a "funnel effect" that naturally guides walkers from bottom to top, making this suitable for studying directed transport phenomena.

## ✨ Features

### Lattice Generator
- Generate M×N honeycomb lattices with customizable dimensions
- Probabilistic coloring for interior hexagons
- Deterministic boundary conditions for edge hexagons
- Export lattice as numpy array for computational processing
- Clean matplotlib visualization

### Random Walk Simulator
- Walker moves along hexagon edges (not through centers)
- Color-based turning rules create emergent path behavior
- Automatic coordinate cleaning to prevent floating-point drift
- Lazy coloring with caching for consistency
- Natural termination at boundaries
- Path statistics and analysis

## 🚀 Quick Start

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/honeycomb-random-walk.git
cd honeycomb-random-walk

# Install required packages
pip install numpy matplotlib
```

### Basic Usage

```python
from honeycomb_lattice import create_lattice_with_bc
from honeycomb_walk import HoneycombWalk

# Create a honeycomb lattice with boundary conditions
lattice = create_lattice_with_bc(M=40, N=40, p=0.5, seed=42)

# Visualize the lattice
fig, ax = lattice.visualize()

# Get the color matrix
colors = lattice.get_color_matrix()

# Run a random walk on the lattice
walker = HoneycombWalk(M=40, N=40, p=0.5, seed=42)
path = walker.run()

# Get walk statistics
stats = walker.get_statistics()
print(f"Total steps: {stats['total_steps']}")
print(f"Final position: {stats['final_position']}")
```

## 🎯 Boundary Conditions

The lattice implements specific boundary conditions that create a natural funnel effect:

| Boundary | Color Rule |
|----------|------------|
| **Left** (col=0) | All GRAY |
| **Right** (col=N-1) | All WHITE |
| **Bottom** (row=0) | GRAY if x < 0, WHITE if x ≥ 0 |
| **Top** (row=M-1) | GRAY if x < x_split, WHITE if x ≥ x_split |

Where `x_split = ((M-1) % 2) × (√3/2)`

These conditions guide walkers from the bottom-middle entrance to the top-middle exit.

## 📊 Example Visualizations

### Honeycomb Lattice
- M×N grid of hexagons
- Gray hexagons (probability p)
- White hexagons (probability 1-p)
- Fixed boundary colors

### Random Walk Path
- Blue path trajectory
- Green start point
- Red end point
- Natural termination at boundaries

## 🔬 Applications

This simulator can model various physical and mathematical systems:

- **Particle Diffusion**: Transport in structured media with random scatterers
- **Optical Propagation**: Light rays through random optical media
- **Percolation Theory**: Flow through porous materials
- **Brownian Motion**: Directional bias in confined geometries
- **Material Science**: Defect migration in crystalline structures
- **Game Theory**: Spatial strategies on hexagonal grids

## 📐 Mathematical Background

### Hexagonal Lattice Geometry
- **Lattice Type**: Regular hexagonal tiling (honeycomb)
- **Coordination**: Each vertex connects 3 edges
- **Movement**: 6 possible directions at 60° intervals
- **Edge Length**: Unit distance

### Random Walk Dynamics
- **State Space**: Position (x,y) and direction (0-5)
- **Transition Rules**: Deterministic based on hexagon color
- **Boundary Behavior**: Natural termination via funnel effect
- **Path Properties**: Self-avoiding in expectation

### Coordinate System
- **Origin**: Bottom-middle edge between central hexagons
- **X-axis**: Horizontal (increases rightward)
- **Y-axis**: Vertical (increases upward)
- **Hexagon Centers**: Calculated with proper honeycomb geometry

## 🔧 Advanced Features

### Color Matrix Representation
```python
# Get M×N matrix (1=gray, 0=white)
colors = lattice.get_color_matrix()

# Matrix structure:
# - colors[0] = bottom row of lattice
# - colors[M-1] = top row of lattice
# - colors[:, 0] = left column (all 1s)
# - colors[:, -1] = right column (all 0s)
```

### Custom Probability Distributions
```python
# Vary gray probability
for p in [0.3, 0.5, 0.7]:
    walker = HoneycombWalk(M=40, N=40, p=p)
    path = walker.run()
    # Analyze path properties vs p
```

### Reproducible Simulations
```python
# Fixed seed for reproducibility
seed = 12345
lattice = create_lattice_with_bc(M=30, N=30, p=0.5, seed=seed)
walker = HoneycombWalk(M=30, N=30, p=0.5, seed=seed)
```

## 📁 Repository Structure

```
percolation/
│
├── honeycomb_lattice.ipynb    # Lattice generator with boundary conditions
├── honeycomb_walk.ipynb       # Random walk simulator
└── README.md                  # This file
```

## 📦 Requirements

- Python 3.6+
- NumPy >= 1.19.0
- Matplotlib >= 3.3.0

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

## 📈 Performance Considerations

- **Memory**: O(M×N) for lattice storage
- **Time Complexity**: O(steps) for walk simulation
- **Coordinate Precision**: Automatic cleaning prevents drift
- **Caching**: Lazy coloring reduces redundant calculations

## 🔮 Future Enhancements

- [ ] 3D honeycomb lattices
- [ ] Multiple simultaneous walkers
- [ ] Interactive visualization
- [ ] GPU acceleration for large-scale simulations
- [ ] Statistical analysis tools
- [ ] Export to common graph formats

## 📚 References

1. Random Walks on Hexagonal Lattices - [relevant paper]
2. Percolation Theory and Applications - [textbook reference]
3. Honeycomb Lattice Geometry - [mathematical reference]

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👥 Authors

- Tayeb jamali - [GitHub Profile](https://github.com/tjamali)
