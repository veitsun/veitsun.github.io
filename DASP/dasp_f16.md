### 数据布局

FP 16 半精度浮点数， 在硬件中通常以 16 位表示， 但 tensor core 通过 uint32_t 操作 32 位宽度，处理两个连续的 FP16 的值。

### mma_m8n8k4_fp16

mma.sync.aligned.m8n8k4.row.col.f16.f16.f16.f16 { C }, { A }, { B }, { C };

- `mma.sync`：表示同步的矩阵乘加。
- `aligned.m8n8k4`：表示矩阵的形状和对齐要求。
  - 8×4: 矩阵 `A` 的形状。
  - 4×8: 矩阵 `B` 的形状。
  - 8×8: 矩阵 `C` 的形状。
- `row.col`：表示 `A` 是按行存储的，`B` 是按列存储的。
- `f16.f16.f16.f16`：表示所有输入和输出数据都是半精度（FP16）。
- `{ C }`：累加矩阵。
- `{ A }`：左乘矩阵。
- `{ B }`：右乘矩阵。

操作说明

1. 从 `frag_a` 和 `frag_b` 中提取矩阵 `A` 和 `B` 的元素。
2. 在 Tensor Core 上完成 C+=A⋅B 的操作。
3. 结果存储在 `C` 中
