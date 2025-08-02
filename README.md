# Synchronized Video Recorder for Android
This work is based on [Sub-millisecond Video Synchronization of Multiple Android Smartphones](https://arxiv.org/abs/2107.00987)

## Usage:
### Leader smartphone setup
1. Insert a SIM card.
2. Disable WiFi.
2. Enable the mobile hotspot.
4. The app should display connected clients and buttons for recording control.

### Client smartphones setup
1. Enable WiFi.
2. Connect to the hotspot from leader.

### Recording video
1. Press the ```calculate period``` button. The app will analyze the frame stream and use the calculated frame period in subsequent synchronization steps.
2. Adjust exposure and ISO as needed.
3. Press the ```phase align``` button and wait until the ```Phase Error``` showed on each device converges.
4. Press the ```record video``` button to start the synchronized video recording.
5. Press the ```record video``` button again to stop recording.
6. Restart the app before each recording.
7. Connect the phone to a computer and get videos from RecSync folder in smartphone root directory.
8. Process the output with [Multiview-Motion-Capture-Data-Processing](https://github.com/yubohuangai/Multiview-Motion-Capture-Data-Processing)

## Contribution

### Stable and Consistent Video Encoding with High Profile H.264

#### Problem
MediaRecorder may automatically select different H.264 encoding profiles across devices, even for the same phone model. This inconsistency can result in excessively high bitrates, which slow down the encoding process and lead to frame drops.
#### Solution
By replacing MediaRecorder with MediaCodec and MediaMuxer, and explicitly setting the H.264 codec to High Profile, we ensure uniform encoder behavior across devices and drop-free video recording.

---
### Accurate Timestamp Logging for All Encoded Frames

#### Problem
Timestamps were previously logged only during onCaptureCompleted and only while isVideoRecording=true. However, the camera may continue encoding a few trailing frames after isVideoRecording is set to false. These final frames were being recorded without timestamps.
#### Solution
We fixed this by delaying the logger shutdown. Specifically, we moved the cleanup logic to occur after releasing the encoder and muxer in startDrainingEncoder().

## Reference
[Sub-millisecond Video Synchronization of Multiple Android Smartphones](https://arxiv.org/abs/2107.00987)

