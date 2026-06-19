# 🛡️ SPECTRASHIELD

### Dual-Engine Hyperspectral Anomaly Detection System







> **SPECTRASHIELD v2.1** — Unsupervised, real-time hyperspectral anomaly detection powered by a dual-engine fusion pipeline. Deployed on Cloudflare Workers for low-latency, globally distributed inference.

***

## 🌐 Live Demo

🔗 [https://spectrashield.vaishnavithamma.workers.dev/](https://spectrashield.vaishnavithamma.workers.dev/)

***

## 📌 Overview

**SPECTRASHIELD** is a web-based hyperspectral anomaly detection tool that processes hyperspectral image cubes and identifies anomalous pixels using a dual-engine fusion approach. It requires no labelled training data (fully unsupervised) and delivers inference results in under 3 seconds.

Hyperspectral images capture hundreds of spectral bands per pixel, enabling detection of anomalies invisible to standard RGB cameras — useful in remote sensing, agriculture, mineralogy, surveillance, and quality inspection.

***

## ✨ Key Features

- **Dual-Engine Fusion** — Combines two complementary anomaly detection algorithms for improved accuracy and reduced false alarms
- **Unsupervised Detection** — No labeled data or prior spectral signatures required
- **GPU Ready** — Accelerated inference pipeline for fast computation
- **< 3s Inference** — Near real-time processing even for large hyperspectral cubes
- **Multi-Format Input** — Supports `.mat`, `.hdr`, `.img`, and `.npy` file formats
- **Drag & Drop Interface** — Simple browser-based upload with instant visual results
- **Cloudflare Workers Deployment** — Globally distributed, low-latency edge deployment

***

## 🗂️ Supported File Formats

| Format | Description |
|--------|-------------|
| `.mat` | MATLAB matrix files (common in hyperspectral datasets) |
| `.hdr` + `.img` | ENVI standard format (header + binary image pair) |
| `.npy` | NumPy array format (Python-native hyperspectral cubes) |

***

## 🚀 Getting Started

### Prerequisites

- Node.js `>= 18.x`
- [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/) (Cloudflare Workers deployment tool)
- A Cloudflare account (for deployment)
- Python `>= 3.9` (if running the detection backend locally)

### Installation

```bash
# Clone the repository
git clone https://github.com/vaishnavithamma/spectrashield.git
cd spectrashield

# Install dependencies
npm install
```

### Local Development

```bash
# Start local Wrangler dev server
npx wrangler dev
```

The app will be available at `http://localhost:8787`.

### Deploy to Cloudflare Workers

```bash
# Authenticate with Cloudflare
npx wrangler login

# Deploy
npx wrangler deploy
```

***

## 🧠 How It Works

SPECTRASHIELD uses a **dual-engine detection pipeline**:

1. **Engine 1 — Statistical Background Model**
   Builds a global statistical model (e.g., RX / Reed-Xiaoli Detector) of the hyperspectral background. Pixels that deviate significantly from this model are flagged as anomalies based on their Mahalanobis distance.

2. **Engine 2 — Representation-Based Detection**
   Uses low-rank or sparse decomposition to separate the background component from potential anomalous targets. Pixels that cannot be well-reconstructed from the background dictionary are candidates for anomalies.

3. **Fusion**
   The outputs of both engines are fused to produce a final anomaly score map, reducing false positives and improving detection confidence.

```
Input Hyperspectral Cube (.mat / .hdr / .npy)
          │
          ▼
  ┌───────────────┐       ┌─────────────────────────┐
  │  Engine 1     │       │  Engine 2                │
  │  Statistical  │       │  Representation-Based    │
  │  (RX-based)   │       │  (Low-Rank / Sparse)     │
  └──────┬────────┘       └────────────┬────────────┘
         │                             │
         └────────────┬────────────────┘
                      ▼
              Dual-Engine Fusion
                      │
                      ▼
           Anomaly Score Heatmap
```

***

## 📁 Project Structure

```
spectrashield/
├── src/
│   ├── worker.js          # Cloudflare Worker entry point
│   ├── detection/
│   │   ├── engine1.js     # Statistical anomaly engine
│   │   ├── engine2.js     # Representation-based engine
│   │   └── fusion.js      # Dual-engine fusion logic
│   ├── parser/
│   │   ├── matParser.js   # .mat file parser
│   │   ├── enviParser.js  # .hdr/.img ENVI parser
│   │   └── npyParser.js   # .npy NumPy parser
│   └── ui/
│       └── index.html     # Frontend drag-and-drop UI
├── wrangler.toml          # Cloudflare Workers config
├── package.json
└── README.md
```

***

## ⚙️ Configuration

Edit `wrangler.toml` to configure your deployment:

```toml
name = "spectrashield"
main = "src/worker.js"
compatibility_date = "2025-01-01"

[build]
command = "npm run build"
```

***

## 🖥️ Usage

1. Open the [live app](https://spectrashield.vaishnavithamma.workers.dev/)
2. Drag and drop a hyperspectral cube file (`.mat`, `.hdr`, `.img`, or `.npy`) onto the upload zone
3. SPECTRASHIELD automatically detects the format and begins processing
4. View the anomaly score heatmap overlaid on a false-colour preview of the image
5. Download the result as a processed anomaly map

***

## 📊 Performance

| Metric | Value |
|--------|-------|
| Inference Time | < 3 seconds |
| GPU Acceleration | ✅ Supported |
| Detection Mode | Unsupervised |
| Deployment | Cloudflare Edge (Global) |
| File Support | `.mat`, `.hdr/.img`, `.npy` |

***

## 🛠️ Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | HTML5, CSS3, Vanilla JS |
| **Backend / Runtime** | Cloudflare Workers (V8 Isolates) |
| **Detection Logic** | JavaScript / WebAssembly (WASM) |
| **Deployment** | Cloudflare Workers via Wrangler |
| **Supported Input** | MATLAB `.mat`, ENVI `.hdr/.img`, NumPy `.npy` |

***

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

```bash
# Fork the repository, then:
git checkout -b feature/your-feature-name
git commit -m "feat: describe your change"
git push origin feature/your-feature-name
# Open a Pull Request
```

Please ensure your code follows the existing style and includes relevant tests.

***

## 📄 License

This project is licensed under the **MIT License**. See [LICENSE](./LICENSE) for details.

***

## 👩‍💻 Deployment


- Deployed at: [spectrashield.vaishnavithamma.workers.dev](https://spectrashield.vaishnavithamma.workers.dev/)
- Built with: Cloudflare Workers + Anthropic Research Systems

***

## 🙏 Acknowledgements

- [Cloudflare Workers](https://workers.cloudflare.com/) — Edge compute platform
- Anthropic Research Systems — AI/ML Research tooling
- Hyperspectral remote sensing research community

