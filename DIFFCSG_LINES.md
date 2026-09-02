# DiffCSG CUDA line rasterization

This branch is based on NVIDIA nvdiffrast v0.4.0 at commit
`253ac4fcea7de5f396371124af597e6cc957bfae`.

nvdiffrast v0.4 removed the OpenGL backend. This branch keeps rasterization
CUDA-only and adds one narrow helper for DiffCSG intersection-edge coverage:

- `rasterize_lines(cuda_ctx, pos, lines, resolution)` accepts an `int32` index
  tensor shaped `[N, 2]`.
- Each line is expanded to a one-pixel-wide screen-space quad and sent through
  the existing CUDA triangle rasterizer.
- Line mode is limited to one batched layer (`pos` shaped `[1, V, 4]`).
- Its output is a detached Boolean-intersection pixel mask. Differentiable line
  endpoints are supplied separately to `antialias()` as degenerate triangles.
- No EGL, OpenGL context, or runtime compilation is used.

No DiffCSG renderer or experiment code belongs in this fork. Keep future
changes here limited to low-level rasterizer support and preserve this branch
as an auditable patch over the upstream tag.
