# Deployment Guide - Quantum Background Removal Engine

## Quick Start

### 1. AWS (H100/A100)
```bash
# Launch instance
aws ec2 run-instances \
    --image-id ami-0abcdef1234567890 \
    --instance-type p4d.24xlarge \
    --key-name your-key

# Setup environment
ssh ubuntu@instance-ip
curl -O https://quantum-engine.ai/setup/aws-h100.sh
chmod +x aws-h100.sh
./aws-h100.sh

# Deploy Quantum Engine
git clone https://github.com/quantum-bgremoval/engine
cd engine
cargo build --release --features gpu
./target/release/quantum-engine --gpu --workers 64
```

### 2. GCP (384-Core CPU)
```bash
# Create instance
gcloud compute instances create quantum-engine \
    --machine-type=c4d-highcpu-384-metal \
    --zone=us-central1-a \
    --image-family=ubuntu-2204-lts \
    --image-project=ubuntu-os-cloud

# Connect and setup
gcloud compute ssh quantum-engine
curl -O https://quantum-engine.ai/setup/gcp-cpu.sh
chmod +x gcp-cpu.sh
./gcp-cpu.sh

# Deploy with maximum performance
export RUSTFLAGS='-C target-cpu=native'
cargo build --release
./target/release/quantum-engine --workers 384
```

### 3. Azure (TPU/GPU)
```bash
# Create VM
az vm create \
    --resource-group quantum-rg \
    --name quantum-engine \
    --image Ubuntu2204 \
    --size Standard_ND96asr_v4

# Setup
ssh azureuser@vm-ip
curl -O https://quantum-engine.ai/setup/azure-a100.sh
chmod +x azure-a100.sh
./azure-a100.sh
```

## Docker Deployment

### Pre-built Images
```bash
# CPU Version (384-core optimized)
docker pull quantumengine/bgremoval:cpu-latest

# GPU Version (CUDA 12.8)
docker pull quantumengine/bgremoval:gpu-latest

# Run CPU version
docker run -d \
    -p 8080:8080 \
    -v /path/to/models:/models \
    -v /path/to/images:/images \
    quantumengine/bgremoval:cpu-latest \
    --workers $(nproc) \
    --model /models/inspyrenet_fast.onnx

# Run GPU version
docker run -d \
    --gpus all \
    -p 8080:8080 \
    -v /path/to/models:/models \
    quantumengine/bgremoval:gpu-latest \
    --gpu \
    --workers 64
```

### Build Your Own
```dockerfile
FROM ubuntu:22.04

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    pkg-config \
    libssl-dev \
    curl

# Install Rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"

# Copy source
COPY . /app
WORKDIR /app

# Build with native optimizations
RUN RUSTFLAGS='-C target-cpu=native' cargo build --release

# Runtime
EXPOSE 8080
ENTRYPOINT ["./target/release/quantum-engine"]
```

## Kubernetes Deployment

### Helm Chart
```bash
# Add repository
helm repo add quantum https://charts.quantum-engine.ai
helm repo update

# Install with custom values
helm install quantum-engine quantum/bgremoval \
    --set workers=384 \
    --set model.path=/models/custom.onnx \
    --set autoscaling.enabled=true \
    --set autoscaling.minReplicas=1 \
    --set autoscaling.maxReplicas=10
```

### Manual Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: quantum-engine
spec:
  replicas: 3
  selector:
    matchLabels:
      app: quantum-engine
  template:
    metadata:
      labels:
        app: quantum-engine
    spec:
      containers:
      - name: engine
        image: quantumengine/bgremoval:cpu-latest
        args:
          - --workers
          - "384"
          - --model
          - /models/inspyrenet_fast.onnx
        resources:
          requests:
            cpu: "384"
            memory: "768Gi"
          limits:
            cpu: "384"
            memory: "768Gi"
        volumeMounts:
        - name: models
          mountPath: /models
      volumes:
      - name: models
        configMap:
          name: quantum-models
---
apiVersion: v1
kind: Service
metadata:
  name: quantum-engine
spec:
  selector:
    app: quantum-engine
  ports:
  - port: 8080
    targetPort: 8080
  type: LoadBalancer
```

## Environment Variables

```bash
# Core Configuration
export QUANTUM_WORKERS=384           # Number of worker threads
export QUANTUM_MODEL=/path/to/model  # ONNX model path
export QUANTUM_BATCH_SIZE=32         # Batch size for inference

# Performance Tuning
export QUANTUM_NUMA_AWARE=true       # Enable NUMA optimizations
export QUANTUM_PREFETCH=true         # Enable memory prefetching
export QUANTUM_SIMD=avx512          # SIMD instruction set

# GPU Configuration
export QUANTUM_GPU=true              # Enable GPU acceleration
export CUDA_VISIBLE_DEVICES=0,1,2,3  # GPU device IDs
export ORT_DISABLE_CPU_EP_FALLBACK=1 # Force GPU-only execution

# Monitoring
export QUANTUM_METRICS=true          # Enable metrics endpoint
export QUANTUM_METRICS_PORT=9090     # Metrics port
```

## BYOM (Bring Your Own Model)

### Model Requirements
- Format: ONNX 1.14+
- Input: 320x320 RGB images
- Output: Single-channel mask
- Quantization: INT8/FP16/FP32 supported

### Model Integration
```bash
# Convert PyTorch to ONNX
python export_model.py \
    --input model.pth \
    --output custom_model.onnx \
    --input-size 320

# Optimize for deployment
python optimize_onnx.py \
    --input custom_model.onnx \
    --output optimized_model.onnx \
    --quantize int8

# Deploy with custom model
./quantum-engine \
    --model optimized_model.onnx \
    --workers 384
```

## Monitoring & Observability

### Prometheus Metrics
```yaml
# prometheus.yml
scrape_configs:
  - job_name: 'quantum-engine'
    static_configs:
      - targets: ['localhost:9090']
```

### Available Metrics
- `quantum_throughput_images_per_second`
- `quantum_latency_percentiles`
- `quantum_memory_usage_bytes`
- `quantum_worker_utilization`
- `quantum_model_inference_time_ms`

### Grafana Dashboard
Import dashboard ID: `15234` from grafana.com

## Troubleshooting

### Common Issues

1. **Low Performance**
   ```bash
   # Check NUMA topology
   numactl --hardware
   
   # Pin workers to NUMA nodes
   numactl --cpunodebind=0-7 ./quantum-engine
   ```

2. **Memory Issues**
   ```bash
   # Increase system limits
   ulimit -n 65536
   ulimit -l unlimited
   
   # Enable huge pages
   echo 1024 > /proc/sys/vm/nr_hugepages
   ```

3. **GPU Not Detected**
   ```bash
   # Check CUDA installation
   nvidia-smi
   
   # Verify ONNX Runtime GPU
   python -c "import onnxruntime as ort; print(ort.get_available_providers())"
   ```

## Support

- Documentation: https://docs.quantum-engine.ai
- GitHub Issues: https://github.com/quantum-bgremoval/issues
- Enterprise Support: enterprise@quantumengine.ai
- Community Discord: https://discord.gg/quantum-engine