# NetMoE

This repository is the open-source implementation of our EuroSys '27 paper:

> **NetMoE: Orchestrating Fine-Grained Overlap for Efficient Distributed MoE Inference**

NetMoE is a fine-grained orchestration framework for distributed Mixture-of-Experts (MoE) inference. It pipelines expert-parallel communication (dispatch/combine) with expert computation at fine granularity, hiding the communication latency of all-to-all exchanges behind overlapped GEMM execution and thereby improving end-to-end throughput of large-scale MoE serving.

## Repository Structure

The core of NetMoE lives in three forked and modified libraries, integrated as git submodules under `thirdparty/`:

| Directory | Upstream | Description |
|---|---|---|
| [`thirdparty/DeepEP`](https://github.com/NASA-NJU/NetMoE-DeepEP) | [deepseek-ai/DeepEP](https://github.com/deepseek-ai/DeepEP) | MoE dispatch/combine communication kernels, modified to expose fine-grained, chunked all-to-all primitives that enable overlap with expert computation. |
| [`thirdparty/DeepGEMM`](https://github.com/NASA-NJU/NetMoE-DeepGEMM) | [deepseek-ai/DeepGEMM](https://github.com/deepseek-ai/DeepGEMM) | High-performance FP8 GEMM kernels, modified with grouped-GEMM interfaces for executing partial token chunks as communication progresses. |
| [`thirdparty/SGLang`](https://github.com/NASA-NJU/NetMoE-SGLang) | [sgl-project/sglang](https://github.com/sgl-project/sglang) | The serving framework, modified with the NetMoE scheduler and overlap-aware MoE forward path that orchestrates the above kernels. |

Each submodule is pinned to a specific commit to guarantee reproducibility.

## Getting the Code

Clone with all submodules:

```bash
git clone --recursive https://github.com/NASA-NJU/NetMoE.git
```

If you already have a checkout:

```bash
git submodule update --init --recursive
```

## Citation

If you find NetMoE useful in your research, please cite:

```bibtex
@inproceedings{netmoe-eurosys27,
  author    = {TODO},
  title     = {NetMoE: Orchestrating Fine-Grained Overlap for Efficient Distributed MoE Inference},
  booktitle = {Proceedings of the 2027 ACM European Conference on Computer Systems (EuroSys '27)},
  year      = {2027}
}
```

## License

This repository is licensed under the [GNU General Public License v3](LICENSE). The submodules retain their respective upstream licenses.
