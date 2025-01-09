# TU Berlin's Event-based Robot Vision course

🕸️ [Course Webpage](https://sites.google.com/view/guillermogallego/teaching/event-based-robot-vision)

📑 [Course Description](https://drive.google.com/file/d/1oGUnvcIwhG9xNmy1mDmNSSrlRdf9Rhgf/view)

📺 [Youtube Playlist](https://www.youtube.com/playlist?list=PL03Gm3nZjVgUFYUh3v5x8jVonjrGfcal8)

> Participants will learn basic concepts, theoretical foundations and relevant
> algorithms developed in the field of event-based (i.e., neuromorphic) vision.
> Upon completing the module, participants will have an overview of the field,
> spanning from the principle of operation of event-based sensors (e.g.,
> event-based cameras), their advantages and disadvantages, to the methods used
> to process their output for a target application. Participants will also be
> aware of the differences with standard (frame-based) computer vision, in terms
> of methods, performance criteria and applications.

### References

- [Survey paper (overview of the field) - 9MB](https://www.google.com/url?q=https%3A%2F%2Farxiv.org%2Fpdf%2F1904.08405.pdf&sa=D&sntz=1&usg=AOvVaw2SQhZyYjlpKOqIs0UexrOq), Gallego et al., IEEE TPAMI 2020.
- [List of Event-based Vision Resources](https://sites.google.com/view/guillermogallego/teaching/event-based-robot-vision): It collects relevant resources in the
  field: links to information (papers, videos, organizations, companies and
  workshops) as well as drivers, code, datasets, simulators and other essential
  tools in event-based vision.

## 📸 Summary of Event-Based Vision Notes

<!-- mtoc-start -->

* [Key Features of Event-Based Cameras](#key-features-of-event-based-cameras)
  * [Mathematical Model for Event Generation](#mathematical-model-for-event-generation)
  * [Types of Event-Based Sensors](#types-of-event-based-sensors)
  * [Representations of Event Data](#representations-of-event-data)
  * [Event Processing Techniques](#event-processing-techniques)
  * [Applications](#applications)
  * [Advantages and Challenges](#advantages-and-challenges)
  * [Future Directions](#future-directions)

<!-- mtoc-end -->

### Key Features of Event-Based Cameras

- **Core Principle**:
  - Detects changes in light intensity asynchronously for each pixel.
  - Unlike frame-based cameras, outputs data only when a change occurs.
- **Event Data**: Outputs events $e = (x, y, t, p)$, where:
  - $(x, y)$ Pixel coordinates.
  - $t$: Timestamp with $\mu s$ precision.
  - $p$: Polarity ($+1$ for intensity increase, $-1$ for decrease).
- **Advantages**:
  - High temporal resolution ($\sim 1 \mu s$).
  - Low latency and minimal motion blur.
  - High dynamic range (HDR) $\sim 140 \, dB$.
  - Sparse output with reduced data redundancy.
  - Energy-efficient (low power consumption).
  - Real-time capabilities ideal for high-speed applications.

### Mathematical Model for Event Generation

1. **Trigger Condition**:

```math
\Delta L = \log I(x, t) - \log I(x, t - \Delta t) = \pm C
```

- $\Delta L$: Log intensity change.
- $C$: Contrast threshold triggering the event.

2. **Brightness Constancy Equation**:
   - Events align with gradients of intensity changes over time:

```math
\frac{dL}{dt} = \nabla L \cdot \mathbf{v} + \frac{\partial L}{\partial t} = 0,
```

where $\mathbf{v}$ Apparent velocity of intensity changes on the image plane.

3. **Noise Model**:
   - Event generation is affected by probabilistic noise:

```math
\Delta L \sim \mathcal{N}(C, \sigma^2),
```

where $\sigma^2$ represents noise variance.

### Types of Event-Based Sensors

1. **Dynamic Vision Sensor (DVS)**:
   - Specialized in detecting temporal contrast events.
   - Resolution ranges from $128 \times 128$ (early models) to $1 \, MP$ (modern sensors).
2. **Asynchronous Time-based Image Sensor (ATIS)**:
   - Combines DVS-like events with grayscale exposure measurements (EM).
   - Provides temporal and intensity data simultaneously.
3. **Dynamic and Active Pixel Vision Sensor (DAVIS)**:
   - Merges DVS events with standard frame-based imaging.
   - Outputs: DVS events, grayscale frames, and IMU data.

### Representations of Event Data

1. **Point-Based Representations**:
   - **Space-Time Point Clouds**: Represent events in $x, y, t$ space.
   - Retains spatial and temporal structure.
2. **Event Frames**:
   - Converts events into 2D histograms or edge maps:
     - Histograms of event counts.
     - Polarity-based accumulations.
     - Time surfaces ( $T(x, y) = f(\text{time of last event})$ ).
3. **Voxel Grids**:
   - Constructs 3D histograms of events in a space-time volume.
   - Balances memory usage and event fidelity.
4. **Motion-Compensated Frames**:
   - Aligns events based on motion hypotheses for sharper edge representation.
5. **Reconstructed Intensity Images**:
   - Integrates event streams to approximate scene brightness:

```math
\log \hat{I}(x, t) = \log I(x, 0) + \sum_k p_k C \delta(x - x_k, t - t_k).
```

- Applications:
  - HDR video recovery.
  - Scene understanding.

### Event Processing Techniques

1. **Event-by-Event Processing**:
   - Processes each event individually, maintaining microsecond latency.
   - Example Methods:
     - Spiking Neural Networks (SNNs).
     - Bayesian filters for noise reduction and inference.
   - **Pros**: Minimal latency, high temporal precision.
   - **Cons**: Computationally expensive for high event rates.
2. **Event Packets**:
   - Groups $N$ events into packets for batch processing.
   - Aggregation strategies include fixed time intervals or adaptive thresholds.
   - Suitable for real-time tasks with reduced computational overhead.
3. **Hybrid Representations**:
   - Time surfaces and voxel grids combine advantages of event-by-event and packet processing.

### Applications

1. **SLAM (Simultaneous Localization and Mapping)**:
   - Combines event data with inertial measurement units (IMUs) for high-speed, HDR mapping.
2. **Autonomous Systems**:
   - Low-latency, robust detection in dynamic environments (e.g., drones, self-driving cars).
3. **Object Recognition and Tracking**:
   - High-speed motion tracking and gesture recognition.
4. **3D Reconstruction**:
   - Stereo event cameras generate depth maps and high-precision reconstructions.
5. **Microscopy**:
   - Tracks rapid biological dynamics, such as neural activity.

### Advantages and Challenges

**Advantages**:

- Real-time operation with minimal motion blur.
- Wide HDR enables use in low-light or high-contrast scenarios.
- Low data redundancy reduces memory and bandwidth requirements.

**Challenges**:

- **Algorithm Design**:
  - Asynchronous data necessitates novel approaches incompatible with traditional vision algorithms.
- **Event Noise**:
  - Variability in event generation requires denoising techniques.
- **Integration with Frame-Based Systems**:
  - Hybrid systems face challenges in synchronizing event-based and frame-based data.
- **Hardware Limitations**:
  - Achieving high resolution while retaining low power consumption.

### Future Directions

1. **Algorithmic Development**:
   - Focus on neuromorphic designs mimicking biological systems.
   - Improved reconstruction and feature extraction techniques.
2. **Enhanced Hardware**:
   - Higher resolutions with efficient processing pipelines.
3. **Applications in Computational Imaging**:
   - Integrating event cameras with deep learning for end-to-end system optimization.
