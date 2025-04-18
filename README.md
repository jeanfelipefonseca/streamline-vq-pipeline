# Streamline Video Quality Pipeline

A comprehensive framework for video quality analysis and optimization, focusing on objective and subjective metrics and adaptive streaming encoding ladder strategies.

## Video Quality Metrics

Video quality assessment is fundamental for optimizing user experience and encoding efficiency. This project implements and analyzes both objective and subjective metrics for a comprehensive evaluation.

### Objective Metrics

Objective metrics utilize mathematical algorithms to quantify video quality without human intervention, comparing the processed video with the original.

#### PSNR (Peak Signal-to-Noise Ratio)

PSNR is a metric that compares the difference in pixel values between two images, measuring the ratio between the maximum possible power of the signal (original video) and the peak signal of the noise (compressed video).

```
PSNR = 10log₁₀(MAX²/MSE)
```

Where:
- MAX is the maximum possible pixel value (255 for 8-bit depth)
- MSE is the mean squared error between original and compressed frames

**Key characteristics:**
- Typical values: 30-50 dB for lossy compression (higher values indicate better quality)
- Advantages: Simple to compute, long history of usage, easily applicable for encoding optimization
- Limitations: Does not perfectly correlate with human perception

**Important variants:**
- PSNR avg.MSE: Calculates the arithmetic mean of MSE first and then applies the logarithm
- PSNR avg.log: Calculates PSNR for each frame and then the arithmetic mean (tends to favor high-quality frames)

#### SSIM (Structural Similarity Index Measure)

SSIM is an image quality assessment metric that measures structural similarity between two images, considering three aspects: luminance, contrast, and structure.

```
SSIM(x,y) = (2μₓμᵧ+c₁)(2σₓᵧ+c₂)/(μₓ²+μᵧ²+c₁)(σₓ²+σᵧ²+c₂)
```

Where:
- μₓ, μᵧ are the means of x and y
- σₓ², σᵧ² are the variances of x and y
- σₓᵧ is the covariance of x and y
- c₁, c₂ are constants to stabilize division

**Key characteristics:**
- Value range: [0,1], where 1 indicates perfect similarity
- Advantages: Better correlation with human perception than PSNR, captures structural distortions
- Applications: Content-dependent distortion estimation, noise impact measurement, blurring artifacts capture

#### VMAF (Video Multi-Method Assessment Fusion)

VMAF is an objective video quality metric developed by Netflix that combines multiple algorithms to predict perceived quality by users.

**Key characteristics:**
- Combines VIF (Visual Information Fidelity), DLM (Detail Loss Metric), and temporal features
- Trained with human subjective evaluation data
- Scale: 0-100, where higher values indicate better quality
- Advantages: High correlation with human perception, considers temporal aspects of video
- Applications: Codec comparison, encoder implementation evaluation, encoding configuration assessment

### Subjective Metrics

Subjective metrics are based on human evaluation of video quality, capturing perceptual aspects that objective metrics may not detect.

#### MOS (Mean Opinion Score)

MOS is a metric that represents the average of quality assessments made by a group of human observers.

**Key characteristics:**
- Scale: 1-5, where 5 represents excellent quality
- Methodology: Observers watch video sequences and assign scores
- Advantages: Directly captures human perception
- Limitations: Time-consuming and costly process, variability among evaluators

#### DMOS (Differential Mean Opinion Score)

DMOS measures the perceived quality difference between the original video and the processed video.

**Key characteristics:**
- Based on the difference between evaluations of the original and processed video
- Reduces variability among evaluators by focusing on relative difference
- Scale: Lower values indicate less perceived degradation

#### JND (Just Noticeable Difference)

JND represents the minimum threshold of change in video quality that can be perceived by a human observer.

**Key characteristics:**
- Measures the sensitivity of the human visual system to quality changes
- Applications: Bitrate optimization, compression threshold determination
- Advantages: Allows identification of the point where additional quality reductions become perceptible

## Encoding Ladders for Adaptive Video Streaming

Encoding ladders are sets of video representations with different resolutions and bitrates, fundamental for adaptive streaming (ABR - Adaptive Bitrate Streaming).

### Encoding Ladder Fundamentals

A well-designed encoding ladder allows the streaming system to automatically select the most appropriate video version based on the user's network conditions and device, ensuring a continuous viewing experience without buffering.

#### Components of an Encoding Ladder

1. **Floor**: The lowest supported bitrate, typically between 145-500 kbps
2. **Ceiling**: The highest offered bitrate, typically 5-8 Mbps for 1080p
3. **Intermediate bitrates**: Levels between floor and ceiling, with increments of 1.5-2x between levels
4. **Resolutions**: Video dimensions appropriate for each bitrate level

#### Example Encoding Ladder (Apple HLS)

| Resolution   | Bitrate (kbps) | Frame rate     |
|--------------|----------------|----------------|
| 416 x 234    | 145            | ≤ 30 fps       |
| 640 x 360    | 365            | ≤ 30 fps       |
| 768 x 432    | 730            | ≤ 30 fps       |
| 768 x 432    | 1100           | ≤ 30 fps       |
| 960 x 540    | 2000           | Original       |
| 1280 x 720   | 3000           | Original       |
| 1280 x 720   | 4500           | Original       |
| 1920 x 1080  | 6000           | Original       |
| 1920 x 1080  | 7800           | Original       |

### Advanced Encoding Ladder Strategies

#### Per-Title Encoding

Per-Title Encoding (also known as content-aware encoding) creates a unique encoding ladder for each video, optimizing quality and efficiency based on content complexity.

**Benefits:**
- Better bitrate allocation based on content complexity
- Bandwidth savings for simple content
- Improved quality for complex content

#### AI-Optimized Encoding Ladders

Modern systems utilize artificial intelligence to dynamically optimize encoding ladders:

- Automatic analysis of content complexity
- Prediction of perceived quality for different configurations
- Dynamic adjustment of encoding parameters

#### Considerations for Different Use Cases

**Low-Latency Streaming:**
- Encoding ladders optimized to minimize delay
- Smaller segments and specific GOP configurations
- Balance between quality and encoding speed

**High-Quality Streaming:**
- Focus on higher resolutions (1080p, 4K)
- Use of advanced codecs (AV1, HEVC)
- Bitrate allocation strategies to maximize perceived quality

## Encoding Ladder Analysis for Adaptive Streaming

Encoding ladder analysis involves the systematic evaluation of different video representations' performance across various network scenarios and devices.

### Analysis Methodology

1. **Quality Assessment:**
   - Application of objective metrics (PSNR, SSIM, VMAF) for each ladder level
   - Subjective tests to validate quality perception

2. **Efficiency Analysis:**
   - Quality-bitrate relationship (RD curves - Rate-Distortion)
   - Comparison of different codecs and configurations

3. **Adaptability Testing:**
   - Simulation of variable network conditions
   - Analysis of frequency and impact of quality changes

### Tools and Frameworks

This project implements tools for:
- Automated generation of encoding ladders
- Comparative analysis of different encoding strategies
- Visualization of quality metrics over time
- Data-driven recommendations for encoding ladder optimization

## Contributions

Contributions are welcome! Please refer to the contribution guidelines for more information.

## License

This project is licensed under [insert license].
