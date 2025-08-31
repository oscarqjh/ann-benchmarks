# ANN-Benchmarks CLI Examples & Testing Guide

This guide documents all the CLI commands used to test and verify the ann-benchmarks project functionality.

## 🚀 **Quick Start**

### 1. Activate Virtual Environment

```bash
# Windows PowerShell
.venv\Scripts\Activate.ps1

# Linux/Mac
source .venv/bin/activate
```

### 2. Verify Installation

```bash
# Run unit tests to ensure basic functionality
python -m pytest test/ -v

# List all available algorithms
python run.py --list-algorithms
```

## 📋 **Core CLI Commands**

### **Basic Command Structure**

```bash
python run.py [OPTIONS]
```

### **Essential Options**

- `--dataset NAME` - Specify dataset (default: glove-100-angular)
- `--algorithm NAME` - Run specific algorithm only
- `--local` - Run locally without Docker
- `--max-n-algorithms N` - Limit number of algorithms to run
- `--run-disabled` - Include disabled algorithms
- `-k COUNT` - Number of nearest neighbors to search (default: 10)
- `--runs COUNT` - Number of runs per algorithm (default: 5)

## 🧪 **Testing Examples**

### **1. Unit Tests**

```bash
# Run all unit tests
python -m pytest test/ -v

# Expected output: 12 tests should pass
```

### **2. Algorithm Discovery**

```bash
# List all available algorithms
python run.py --list-algorithms

# Shows 60+ algorithms with supported metrics and data types
```

### **3. Single Algorithm Testing**

#### **Bruteforce Algorithm (Euclidean)**

```bash
python run.py \
  --dataset random-xs-20-euclidean \
  --algorithm bruteforce-blas \
  --local \
  --max-n-algorithms 1
```

#### **Bruteforce Algorithm (Angular)**

```bash
python run.py \
  --dataset random-xs-20-angular \
  --algorithm bruteforce-blas \
  --local \
  --max-n-algorithms 1
```

#### **BallTree Algorithm**

```bash
python run.py \
  --dataset random-xs-20-euclidean \
  --algorithm ball \
  --local \
  --max-n-algorithms 1 \
  --run-disabled
```

#### **KDTree Algorithm**

```bash
python run.py \
  --dataset random-xs-20-euclidean \
  --algorithm kd \
  --local \
  --max-n-algorithms 1 \
  --run-disabled
```

### **4. Multiple Algorithm Benchmarking**

```bash
# Run 3 algorithms on the same dataset
python run.py \
  --dataset random-xs-20-euclidean \
  --local \
  --max-n-algorithms 3 \
  --run-disabled
```

### **5. Real Dataset Benchmarking**

```bash
# Run on GloVe dataset (1.1M vectors, 100 dimensions)
python run.py \
  --dataset glove-100-angular \
  --algorithm bruteforce-blas \
  --local \
  --max-n-algorithms 1
```

## 📊 **Available Datasets**

### **Small Test Datasets**

- `random-xs-20-euclidean` - 10K vectors, 20 dimensions
- `random-xs-20-angular` - 10K vectors, 20 dimensions
- `random-xs-16-hamming` - 10K bit vectors, 16 dimensions

### **Real-World Datasets**

- `glove-100-angular` - 1.1M word vectors, 100 dimensions
- `mnist-784-euclidean` - 70K image vectors, 784 dimensions
- `sift-128-euclidean` - 1M image feature vectors, 128 dimensions
- `fashion-mnist-784-euclidean` - 70K fashion image vectors, 784 dimensions

## 🔧 **Working Algorithms (No Additional Dependencies)**

### **Basic Algorithms**

- `bruteforce-blas` - Brute force search with BLAS optimization
- `ball` - BallTree from scikit-learn
- `kd` - KDTree from scikit-learn
- `ckdtree` - CKDTree from scipy

### **Algorithm Characteristics**

| Algorithm         | Distance Metrics                     | Status                 | Notes                      |
| ----------------- | ------------------------------------ | ---------------------- | -------------------------- |
| `bruteforce-blas` | euclidean, angular, hamming, jaccard | ✅ Enabled             | Fastest for small datasets |
| `ball`            | any                                  | ⚠️ Disabled by default | Use `--run-disabled`       |
| `kd`              | any                                  | ⚠️ Disabled by default | Use `--run-disabled`       |
| `ckdtree`         | any                                  | ⚠️ Disabled by default | Use `--run-disabled`       |

## ❌ **Algorithms Requiring Additional Dependencies**

These algorithms exist but require packages not in the base requirements:

- `annoy` - Requires `annoy` package
- `faiss` - Requires `faiss` package
- `hnswlib` - Requires `hnswlib` package
- `nmslib` - Requires `nmslib` package
- `qdrant` - Requires `qdrant-client` package
- `milvus` - Requires `pymilvus` package
- `weaviate` - Requires `weaviate` package

## 🐛 **Troubleshooting**

### **Common Issues**

#### **1. "Nothing to run" Error**

```bash
# Problem: All algorithms are disabled
# Solution: Use --run-disabled flag
python run.py --dataset random-xs-20-euclidean --algorithm ball --local --run-disabled
```

#### **2. Module Import Errors**

```bash
# Problem: Algorithm requires additional package
# Solution: Install the required package or use different algorithm
pip install annoy  # For annoy algorithm
pip install faiss  # For faiss algorithms
```

#### **3. Dataset Download Issues**

```bash
# Problem: Cannot download dataset from ann-benchmarks.com
# Solution: Project automatically creates local dataset
# This is normal behavior for test datasets
```

### **Error Messages & Solutions**

| Error Message                              | Cause                        | Solution                                    |
| ------------------------------------------ | ---------------------------- | ------------------------------------------- |
| `Exception: Nothing to run`                | All algorithms disabled      | Add `--run-disabled` flag                   |
| `ModuleNotFoundError: No module named 'X'` | Missing dependency           | Install package or use different algorithm  |
| `HTTP Error 404: Not Found`                | Dataset not available online | Project creates local dataset automatically |

## 📈 **Performance Expectations**

### **Small Datasets (10K vectors)**

- **Index Build Time**: 0.001-0.040 seconds
- **Query Time**: 0.001-0.010 seconds per query
- **Memory Usage**: 72-164 bytes index size

### **Large Datasets (1M+ vectors)**

- **Index Build Time**: 0.3-1.0 seconds
- **Query Time**: 0.001-0.100 seconds per query
- **Memory Usage**: 8+ bytes index size

## 🔍 **Advanced Usage**

### **Custom Parameters**

```bash
# Search for 20 nearest neighbors instead of 10
python run.py \
  --dataset random-xs-20-euclidean \
  --algorithm bruteforce-blas \
  --local \
  -k 20
```

### **Multiple Runs**

```bash
# Run each algorithm 10 times instead of 5
python run.py \
  --dataset random-xs-20-euclidean \
  --algorithm bruteforce-blas \
  --local \
  --runs 10
```

### **Batch Processing**

```bash
# Process all queries at once (may be faster)
python run.py \
  --dataset random-xs-20-euclidean \
  --algorithm bruteforce-blas \
  --local \
  --batch
```

## 📝 **Output Interpretation**

### **Successful Run Output**

```
Trying to instantiate ann_benchmarks.algorithms.bruteforce.BruteForceBLAS(['euclidean'])
Got a train set of size (9000 * 20)
Got 1000 queries
Built index in 0.0009753704071044922
Index size:  704.0
Running query argument group 1 of 1...
Run 1/5...
Processed 1000/1000 queries...
...
Run 5/5...
Processed 1000/1000 queries...
```

### **Key Metrics**

- **Index Build Time**: Time to construct the search index
- **Index Size**: Memory usage of the index in bytes
- **Query Processing**: Number of queries processed per run
- **Run Count**: Number of benchmark runs completed

## 📊 **Viewing Results**

### **Where Results Are Stored**

After running benchmarks, results are stored in the `results/` directory:

```
results/
├── dataset-name/
│   └── k-value/
│       └── algorithm-name/
│           └── metric.hdf5
└── dataset-name.png  # Generated plots
```

### **Raw Results (HDF5 Files)**

Each algorithm run creates an HDF5 file containing:

```bash
# View HDF5 file contents
python -c "
import h5py
f = h5py.File('results/random-xs-20-euclidean/10/bruteforce-blas/euclidean.hdf5', 'r')
print('Keys:', list(f.keys()))
print('Attributes:', dict(f.attrs))
print('Times shape:', f['times'].shape)
print('Neighbors shape:', f['neighbors'].shape)
print('Distances shape:', f['distances'].shape)
"
```

**HDF5 File Contents:**

- `times` - Query execution times for each run
- `neighbors` - Indices of nearest neighbors found
- `distances` - Distances to nearest neighbors
- `attrs` - Metadata (build time, index size, algorithm name, etc.)

### **Generated Plots**

Create performance comparison plots:

```bash
# Generate plot for specific dataset
python plot.py --dataset random-xs-20-euclidean --count 10

# Generate plot for GloVe dataset
python plot.py --dataset glove-100-angular --count 10

# Generate plot for MNIST dataset
python plot.py --dataset mnist-784-euclidean --count 10
```

**Plot Output:**

- **X-axis**: Recall (accuracy of nearest neighbor search)
- **Y-axis**: Queries per second (speed)
- **Lines**: Each algorithm's performance curve
- **File**: Saved as `results/dataset-name.png`

### **Existing Plots**

The project comes with pre-generated plots for major datasets:

- `glove-100-angular.png` - Word vector benchmarks
- `mnist-784-euclidean.png` - Image vector benchmarks
- `sift-128-euclidean.png` - Feature vector benchmarks
- `fashion-mnist-784-euclidean.png` - Fashion image benchmarks

### **Result Analysis**

**Performance Metrics:**

- **Recall**: How many true nearest neighbors were found
- **Queries/Second**: Search speed
- **Build Time**: Index construction time
- **Index Size**: Memory usage
- **Query Time**: Average search time per query

**Example Results:**

```
Computing knn metrics
  0: BallTree(leaf_size=1000)        1.000     3539.464
  1: BallTree(leaf_size=20)          1.000     5814.415
  2: BruteForceBLAS()                1.000     8134.045
  3: CKDTree(leaf_size=40)           1.000    22404.752
  4: KDTree(leaf_size=1000)          1.000     6366.640
  5: KDTree(leaf_size=20)            1.000    11048.138
```

**Interpretation:**

- **Recall 1.000**: Perfect accuracy (found all true nearest neighbors)
- **Numbers**: Queries per second (higher = faster)

## 🎯 **Best Practices**

1. **Start Small**: Begin with small test datasets
2. **Use Local Mode**: Use `--local` for development/testing
3. **Enable Disabled**: Use `--run-disabled` for basic algorithms
4. **Monitor Resources**: Large datasets can be memory-intensive
5. **Check Dependencies**: Verify required packages for advanced algorithms
6. **Generate Plots**: Always create visualizations for performance comparison
7. **Analyze Results**: Look at both accuracy (recall) and speed (queries/second)

## 📚 **Additional Resources**

- **Project Documentation**: See main README.md
- **Algorithm Details**: Check `ann_benchmarks/algorithms/*/config.yml`
- **Dataset Information**: See `ann_benchmarks/datasets.py`
- **Unit Tests**: Explore `test/` directory for examples
- **Result Analysis**: Use `plot.py` for performance visualization

---

**Note**: This guide covers the basic functionality that works out-of-the-box. For advanced algorithms, you may need to install additional dependencies or use Docker containers.
