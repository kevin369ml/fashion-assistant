# fashion-assistant
Image + text > try-on video

## 🔍 Comparison of Open-Source Video Generation Models (Image + Text → Short Video)

| Model | Variant / Parameters | Runtime (3s @ 720×1280)* | Performance (public signals) | Architecture |
|:------|:---------------------|:--------------------------|:-----------------------------|:--------------|
| **STIV** | Base: **8.7B** | N/A (not reported) | VBench T2V **83.1** @512; I2V SOTA @512; supports **TI2V (text+image→video)** | Diffusion Transformer (DiT) |
| **Step-Video-TI2V** | Base: **30B** | N/A | State-of-the-art for text-driven **image+text→video** (up to 102 frames) | Diffusion Transformer |
| **HunyuanVideo** | *Lite*: **3.6B** • *Base*: **13B** • *Ultra*: **28B** | ~4 min for 5s clip (on A100, indicative only) | Competitive with leading closed models; joint image+video training | Diffusion Transformer + 3D VAE |
| **CogVideoX (I2V)** | *Lite*: **2B** • *Base*: **5B** • *Ultra*: **17B** | N/A | Explicit **image + text prompt** conditioning; ~6s @ 720×480 supported | Diffusion Transformer + 3D causal VAE |
| **Wan 2.1** | *Lite*: **1.3B** • *Base*: **7B** • *Pro*: **14B** | N/A | Strong open-source suite; supports both T2V and I2V; active community | xDiT-based diffusion (video DiT) |
| **Mochi 1** | Base: **10B** | N/A | SOTA-level text→video; high prompt fidelity, temporal stability | Asymmetric DiT (Diffusion Transformer) |
| **VideoCrafter 2** | *Base*: **6B** (T2V) • *I2V*: **7B** | N/A | Supports both Text2Video and Image2Video; good mid-quality open baseline | Latent Diffusion (SD lineage) + Temporal modules |
| **Stable Video Diffusion (SVD)** | *1.1*: ≈**1B** • *XT*: ≈**2B** | ~1–2 min per 3s clip @ 576×1024 (A100) | Good temporal consistency; SVD-XL improves realism | Latent Video Diffusion (Stable Diffusion backbone) |
| **Open-Sora 2.0** | *Lite*: **3B** • *Base*: **7B** • *XL*: **11B** | N/A | Approaching Hunyuan/Step-Video quality; open end-to-end research stack | Diffusion Transformer + Efficient VAE |
| **DynamiCrafter** | *Base*: **3.1B** | ~1–2 min for 2s 512p clip (A100) | Optimized for **image+text→video (~2s)**; strong motion smoothness | Latent Diffusion (VideoCrafter lineage) |

\* **Runtime note:** There’s no standardized benchmark (same GPU, same sampling steps) for “3s @ 720×1280”. Reported runtimes are indicative, varying by GPU type, diffusion steps, fps, and scheduler.

---

### 🧩 Summary

- **Best match for image + text → video**: `STIV`, `Step-Video-TI2V`, `DynamiCrafter`
- **High-quality but compute-heavy**: `HunyuanVideo`, `Open-Sora 2.0`
- **Lightweight / deployable options**: `CogVideoX-Lite`, `Wan 2.1-Lite`, `SVD 1.1`
- **Experimental / research-grade**: `VideoCrafter 2`, `Mochi 1`

All of these support or can be adapted for *image + text* conditioning. For production use, you’ll likely want to benchmark:
- **Input flexibility** (480×640, 720×1280)
- **Temporal quality** (motion realism)
- **Inference cost and speed**

## 🔍 Comparison of Open-Source Virtual Try-On (VTON) Models

| Model | GitHub / Link | Model Type / Architecture | Face/Identity Preservation* | Parameters | Runtime / VRAM Info |
|------|--------------|--------------------------|-----------------------------|------------|----------------------|
| **CatVTON** | [github.com/Zheng-Chong/CatVTON](https://github.com/Zheng-Chong/CatVTON) | Diffusion (latent) – concatenation of garment & person images. :contentReference[oaicite:1]{index=1} | ✅ *implicitly* (designed to preserve person image and only transfer garment) | ~**899.06M** total; ~49.57M trainable. :contentReference[oaicite:2]{index=2} | < 8 GB VRAM for 1024×768 resolution. :contentReference[oaicite:3]{index=3} |
| **IDM‑VTON** | [github.com/yisol/IDM-VTON](https://github.com/yisol/IDM-VTON) | Diffusion model (exemplar-based) for virtual try-on in the wild. :contentReference[oaicite:5]{index=5} | ✅ Focuses on garment detail and authenticity; less explicit face loss but preserves source identity well. :contentReference[oaicite:6]{index=6} | *Not publicly specified* | VRAM / runtime not explicitly given; built for “in the wild” use. |
| **OO TDiffusion** | [github.com/levihsu/OOTDiffusion](https://github.com/levihsu/OOTDiffusion) | Latent diffusion model – outfitting fusion in self-attention layers. :contentReference[oaicite:8]{index=8} | ✅ Emphasis on preserving garment and human consistency; face/identity region treated as person image input | *Parameters not clearly listed* | Runtime/VRAM not clearly given |
| **DCI‑VTON** | [github.com/bcmi/DCI-VTON-Virtual-Try-On](https://github.com/bcmi/DCI-VTON-Virtual-Try-On) | Diffusion + warping module; exemplar-based inpainting. :contentReference[oaicite:10]{index=10} | ✅ Claims to “preserve the details of both humans and clothes” (which suggests identity/face preservation) | *Parameters not clearly listed* | Runtime/VRAM not clearly given |
| **VITON (original)** | [github.com/xthan/VITON](https://github.com/xthan/VITON) | GAN-based virtual try-on (two-stage warp + blend). :contentReference[oaicite:12]{index=12} | ⚠️ Basic; older method may distort face/identity more easily | *Parameters not clearly listed* | Runtime/VRAM older generation; high relative to newer diffusion models |

\* “Face/Identity Preservation” here refers to how explicitly the method mentions preserving the person’s facial/identity features (face, hair, body shape) or uses mechanisms (identity loss, face mask, segmentation) to protect them.
