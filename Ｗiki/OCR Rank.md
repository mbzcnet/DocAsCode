**OCR领域权威排行（2026最新，合成OmniDocBench v1.5、OCRBench、CC-OCR等）**\
**维度1：文档解析准确率（OmniDocBench v1.5复杂文档）**

1. GLM-OCR (0.9B, 94.62)
2. PaddleOCR-VL (92.86\~94.5)
3. Qwen3.5-VL (90.8)
4. MinerU2.5 (90.67)
5. Gemini-3 Pro (90.33)

**维度2：纯文本识别准确率（OCRBench / CC-OCR）**

1. Qwen3.5-VL (93.1)
2. GLM-OCR (94.0)
3. PaddleOCR-VL
4. Gemini-3 Pro
5. DeepSeek-OCR

**维度3：速度/效率（轻量部署）**

1. GLM-OCR / PaddleOCR-VL (0.9B)
2. Tesseract / EasyOCR传统
3. Qwen3.5-VL / Qwen3.5-27B/35B
4. dots.ocr
5. olmOCR-2

**维度4：多语言/手写/结构支持**

1. PaddleOCR (90+语言+布局)
2. Qwen3.5-VL
3. GLM-OCR
4. Gemini系列
5. DeepSeek-OCR

**维度5：开源生产就绪度（GitHub/企业采用）**

1. PaddleOCR
2. GLM-OCR
3. Qwen3.5-VL
4. Tesseract
5. MinerU

**备注（2026.2 更新）**：Qwen3.5 为原生多模态 VLM，可作为 OCR/文档解析模型使用；在 OmniDocBench、OCRBench、CC-OCR 等文档/OCR基准上优于 Qwen3-VL（例如 90.8 vs 84.5、93.1 vs 87.5）。因此在本排行中将“Qwen3-VL”统一替换为“Qwen3.5-VL”，最终仍以自家数据验证为准。

**总结**：GLM-OCR 复杂文档SOTA；PaddleOCR-VL 速度+多语言强；Qwen3.5-VL 多模态综合提升大且可单模型覆盖 OCR + 文档解析 + 文本任务（受算力约束时优先）。实际以自家数据验证。
