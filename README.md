# Accelerated Cipher Application Using CUDA Streams

## Aim
The goal of this project is to accelerate encryption and data processing operations using CUDA Streams, enabling concurrent execution of independent tasks on the GPU.

By overlapping computation and data transfer, this solution demonstrates how asynchronous CUDA streams can dramatically improve throughput and GPU utilization.

---

## Overview
This project implements a stream-based CUDA pipeline that:
- Encrypts large data arrays in parallel.  
- Utilizes multiple CUDA streams to overlap data transfers, kernel execution, and memory copies.  
- Demonstrates high concurrency between host-device operations.  

---

## Implementation

### File Structure
- encryption.cuh: Defines the GPU encryption kernel and device-side functions.  
- helpers.cuh: Contains helper macros and functions for CUDA error checking, rounding, and timing.  
- streams_solution.cu: Main CUDA source implementing the multi-stream encryption workflow.  
- Makefile: Automates compilation using nvcc with all dependencies.  

---

## Core Workflow
1. Define the number of CUDA streams.  
2. Create and store the streams in an array.  
3. Split the dataset into equal-sized chunks for each stream using a safe division helper.  
4. For each stream, copy its data chunk to the device, launch the encryption kernel, and copy the results back asynchronously.  
5. Synchronize all streams before validating results on the CPU.  

---

## Profiling and Results
Using CUDA streams with copy/compute overlap reduced total GPU execution time (including memory transfers) from approximately 175 milliseconds to less than 75 milliseconds — achieving more than double the performance compared to the baseline.  

| Version | Streams | Total Time | Speedup |
|----------|----------|-------------|----------|
| Baseline | 1 | ~175 ms | 1× |
| Optimized (Multi-Stream) | 4 | ~74 ms | 2.3× |

Profiler output confirmed substantial overlap between memory copies and kernel execution, resulting in improved GPU utilization and faster overall performance.

---

## CUDA Concepts Demonstrated
- CUDA Streams: Enables asynchronous, concurrent execution of memory transfers and kernels.  
- Copy/Compute Overlap: Achieves concurrency between data movement and computation.  
- Asynchronous Memory Copies: Allows concurrent data transfers and kernel execution.  
- Stream Synchronization: Ensures correctness after concurrent operations complete.  
- Round-up Division Helper: Prevents data loss in uneven chunk divisions.  

---

## Performance Notes
- The optimum number of streams depends on GPU architecture and PCIe bandwidth.  
- Using pinned (page-locked) host memory can further improve transfer speeds.  
- The approach can be scaled for real-time encryption or packet-based data pipelines.  

---

## Future Work
- Use pinned host memory for faster DMA transfers.  
- Integrate shared-memory tiling for higher kernel efficiency.  
- Extend encryption logic to support AES-like block ciphers.  
- Visualize stream concurrency and overlaps using NVIDIA Nsight Systems profiling.  

---

## Author
[Your Name]  
University of Illinois Urbana-Champaign  
Course: Parallel Programming / GPU Optimization  
Instructor: [Instructor’s Name]  
Reference Course: NVIDIA Deep Learning Institute (DLI) — *Fundamentals of Accelerated Computing with CUDA C/C++*

---

## References
- NVIDIA CUDA C Programming Guide  
- CUDA Streams and Concurrency (NVIDIA Developer Blog)  
- GPU Gems 3, Chapter 31: Fast Convolution on GPUs  
- NVIDIA Deep Learning Institute (DLI): Fundamentals of Accelerated Computing with CUDA C/C++  
- Project profiling and performance results
