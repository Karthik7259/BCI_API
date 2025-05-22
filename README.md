# BCI Data Collection and API

This project provides a Brain-Computer Interface (BCI) data collection system with a Flask-based REST API. It processes and streams EEG data from a LEHeadband device, calculating spectral bands (alpha, beta, theta) and providing real-time access to the processed data.

## Features

- Real-time EEG data collection from LEHeadband device
- Processing of raw EEG signals into spectral bands (alpha, beta, theta)
- Approximate Entropy (ApEn) calculation for each band
- REST API endpoint for accessing processed data
- CORS support for frontend integration
- Real-time electrode resistance monitoring
- Automatic calibration process
- CSV logging capability

## Prerequisites

- Python 3.x
- LEHeadband BCI device

### Required Python Packages

```bash
pip install flask flask-cors numpy
```

You also need the following specialized packages:
- `neurosdk` - For communicating with the BCI device
- `em_st_artifacts` - For EEG signal processing

## Project Structure

- `bciapi.py` - Main API server and BCI processing logic
- `emo.py` - Standalone EEG data collection script with CSV logging
- `logs/` - Directory containing SDK logs

## Usage

### Starting the API Server

```bash
python bciapi.py
```

The server will:
1. Initialize the BCI device scanner
2. Search for available LEHeadband devices
3. Perform resistance check on electrodes
4. Start data collection and processing
5. Make processed data available via HTTP endpoint

### API Endpoints

- `GET /api/data` - Returns the latest processed EEG data
  ```json
  {
    "timestamp": "2025-05-22T10:30:45.123456",
    "alpha": 0.75,
    "beta": 0.45,
    "theta": 0.30
  }
  ```

### Standalone Data Collection

For collecting data to CSV without the API server:
```bash
python emo.py
```

This will create a CSV file with detailed EEG metrics and ApEn calculations.

## Data Processing

The system performs the following processing steps:
1. Raw EEG signal acquisition
2. Artifact detection and removal
3. Spectral band calculation (alpha, beta, theta)
4. ApEn calculation for each band
5. Real-time data streaming

## Configuration

Key parameters in the code:
- Sampling rate: 250 Hz
- FFT window: 1000 samples
- Calibration length: 6 seconds
- Buffer size: 20 samples for moving calculations

## Error Handling

The system includes:
- Proper device cleanup on exit
- Exception handling for device connections
- Automatic resource cleanup
- Connection state monitoring

## Frontend Integration

The API includes CORS support for frontend applications. Default configuration allows connections from `localhost:3000`.

## Logs

System logs are stored in `logs/sdk_log.log` for debugging and monitoring purposes.

## Safety Notes

1. Ensure proper electrode contact before starting data collection
2. Monitor electrode resistance values regularly
3. Follow proper device handling and cleaning procedures
4. Stop data collection if any discomfort occurs

## Troubleshooting

1. If no device is found:
   - Ensure the device is powered on
   - Check Bluetooth connectivity
   - Verify device is in range

2. If resistance values are high:
   - Clean electrodes
   - Check electrode placement
   - Ensure proper skin contact

3. If data quality is poor:
   - Wait for calibration to complete
   - Check electrode resistance
   - Minimize movement artifacts
