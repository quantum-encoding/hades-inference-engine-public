# Quantum Background Removal Engine - Enterprise License (BYOM)

## BRING YOUR OWN MODEL (BYOM) COMMERCIAL LICENSE AGREEMENT

**Version 1.0 - Effective Date: January 2025**

### 1. GRANT OF LICENSE

Subject to the terms of this Agreement, Quantum Engine Inc. ("Licensor") grants you ("Licensee") a perpetual, worldwide, non-exclusive license to:

- Deploy the Quantum Background Removal Engine in commercial environments
- Use your own ONNX-compatible models with the engine
- Modify and customize the deployment configuration
- White-label the solution for your enterprise

### 2. BYOM (BRING YOUR OWN MODEL) PROVISIONS

#### 2.1 Model Compatibility
- Licensee may use any ONNX-compatible segmentation model
- Supported model formats: ONNX 1.14+, INT8/FP16/FP32 quantization
- Model size: No restrictions (tested up to 500MB models)

#### 2.2 Model Ownership
- Licensee retains full ownership of their custom models
- No model data is transmitted to Licensor
- Models remain entirely within Licensee's infrastructure

#### 2.3 Performance Guarantees
- Performance scales linearly with hardware (99.8% efficiency)
- Benchmark tools provided for model optimization
- Technical support for model integration available

### 3. DEPLOYMENT OPTIONS

#### 3.1 Unlimited Scaling
- Deploy on any number of servers
- No per-core or per-instance licensing fees
- Support for hybrid CPU/GPU deployments

#### 3.2 Cloud Agnostic
- AWS, GCP, Azure, and on-premises supported
- Kubernetes and Docker deployment scripts included
- Auto-scaling configurations provided

### 4. PRICING TIERS

#### Startup Edition - $4,999/year
- Up to 10 million images/month
- Email support (48hr response)
- Quarterly updates

#### Enterprise Edition - $19,999/year
- Unlimited processing volume
- Priority support (4hr response)
- Monthly updates
- Custom optimization consultation

#### Quantum Edition - Custom Pricing
- White-label rights
- Source code access
- Dedicated engineering support
- Custom feature development
- SLA guarantees

### 5. SUPPORT AND MAINTENANCE

#### 5.1 Included Support
- Installation assistance
- Configuration optimization
- Bug fixes and security patches
- Documentation and best practices

#### 5.2 Additional Services
- Model optimization consulting: $2,500/day
- Custom feature development: $250/hour
- On-site deployment assistance: $5,000/day + expenses

### 6. TECHNICAL REQUIREMENTS

Minimum Hardware:
- 8+ CPU cores (AMD EPYC or Intel Xeon recommended)
- 32GB RAM
- Linux kernel 5.4+
- ONNX Runtime 1.14+

Optimal Hardware:
- 64+ CPU cores
- 128GB+ RAM
- NVMe storage for image I/O
- Optional: NVIDIA GPU with CUDA 11.8+

### 7. BENCHMARK TRANSPARENCY

All performance claims are based on:
- COCO 2017 validation dataset (5,000 images)
- 320x320 input resolution
- Custom InspyreNet model (108MB)
- Full benchmark reproduction steps available

### 8. INTELLECTUAL PROPERTY

#### 8.1 Engine IP
- Licensor retains all rights to the Quantum Engine
- Architecture designs and optimizations remain proprietary
- Work-stealing session pool technology patented

#### 8.2 Licensee IP
- Licensee retains all rights to:
  - Custom models
  - Processed images
  - Custom configurations
  - Integration code

### 9. WARRANTY AND LIABILITY

#### 9.1 Performance Warranty
- Engine will perform as documented
- Linear scaling with hardware verified
- 30-day money-back guarantee

#### 9.2 Limitation of Liability
- Maximum liability limited to license fees paid
- No liability for indirect or consequential damages
- Force majeure provisions apply

### 10. TERMINATION

#### 10.1 Termination Events
- Material breach not cured within 30 days
- Non-payment of license fees
- Violation of intellectual property rights

#### 10.2 Post-Termination
- Licensee may continue using current version
- No further updates or support provided
- Models and processed data remain with Licensee

### 11. COMPLIANCE

#### 11.1 Export Controls
- Licensee responsible for export compliance
- No deployment in embargoed countries
- ITAR/EAR compliance required where applicable

#### 11.2 Data Privacy
- GDPR/CCPA compliant architecture
- No telemetry or usage tracking
- All processing remains on-premises

### 12. CONTACT INFORMATION

**Quantum Engine Inc.**

Sales: sales@quantumengine.ai
Support: support@quantumengine.ai
Legal: legal@quantumengine.ai

Address:
Quantum Engine Inc.
Innovation District
San Francisco, CA 94105
United States

---

*By using the Quantum Background Removal Engine, you agree to these terms.*

**License Key Required for Activation**

To obtain your license key:
1. Complete purchase at https://quantumengine.ai/purchase
2. License key delivered within 1 business day
3. Activate with: `./quantum-engine --license YOUR_KEY`