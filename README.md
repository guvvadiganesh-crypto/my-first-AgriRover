# Agrirover - Advanced IoT Agricultural Dashboard with NPK Analysis

A professional, high-tech mission control dashboard for an autonomous solar-powered agricultural robot. Features real-time telemetry, GPS tracking, comprehensive soil monitoring (pH, moisture, temperature), and **advanced soil nutrient analysis (NPK - Nitrogen, Phosphorus, Potassium)** for Variable Rate Application (VRA) of fertilizers.

## Key Features

✅ **Hero Section** - Agrirover branding with live status indicator
✅ **GPS Field Mapping** - Interactive map showing rover position and field coverage path
✅ **Environmental Sensors** - Real-time gauges for Soil pH, Moisture %, and Temperature
✅ **Soil Nutrient Analysis (NPK)** - Advanced vertical bar charts for Nitrogen, Phosphorus, and Potassium levels (mg/kg)
✅ **Nutrient Alerts** - Real-time notifications when nutrient levels drop below critical thresholds with fertilizer recommendations
✅ **Power Management** - Battery health and solar charge rate visualization  
✅ **Manual Overrides** - Directional pad (Arrow keys) to control DC gear motors
✅ **Data Logging** - Real-time table of sensor readings including NPK values
✅ **CSV Export** - Download complete data logs for analysis
✅ **WebSocket Updates** - Real-time data push using Socket.io (no page refresh needed)
✅ **Emergency Stop** - Red button for immediate rover shutdown
✅ **Modbus RS485 Ready** - Backend prepared for real NPK sensor integration via USB-to-RS485 converter

## Why NPK Analysis Matters

The addition of NPK soil nutrient analysis transforms Agrirover from a basic monitoring system to a precision agriculture platform:

- **Nitrogen (N)**: Essential for plant growth and photosynthesis. Deficiency causes stunted growth and yellowing leaves.
- **Phosphorus (P)**: Critical for energy transfer, root development, and flowering. Low levels reduce yield potential.
- **Potassium (K)**: Improves water retention, disease resistance, and overall plant strength. Deficiency causes weak stems and poor crop quality.

By monitoring these nutrients in real-time, farmers can implement **Variable Rate Application (VRA)** of fertilizers, reducing waste, lowering costs, and minimizing environmental runoff.

### Optimal Ranges (for most crops):
- Nitrogen: 150-300 mg/kg
- Phosphorus: 25-50 mg/kg
- Potassium: 150-300 mg/kg

Alert system triggers when levels fall below safe thresholds.

## Project Structure

```
agrirover/
├── app.py                 # Flask + SocketIO backend
├── requirements.txt       # Python dependencies
├── templates/
│   └── index.html        # Frontend dashboard (HTML/CSS/JS)
└── README.md
```

## Installation

### 1. Clone/Setup Project
```bash
cd agrirover
```

### 2. Create Python Virtual Environment
```bash
python -m venv venv
venv\Scripts\activate  # Windows
# or source venv/bin/activate  # Linux/Mac
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

## Running the Application

### Start the Backend
```bash
python app.py
```

The server will start at `http://localhost:5000`

### Access the Dashboard
Open your browser and navigate to:
```
http://localhost:5000
```

## Features Breakdown

### Backend (app.py)
- **Flask Server**: Serves the dashboard HTML with SocketIO support
- **SocketIO**: Real-time bidirectional WebSocket communication
- **Multi-Sensor Simulation**: 
  - Environmental: pH, Moisture, Temperature
  - Nutritional: Nitrogen (N), Phosphorus (P), Potassium (K) via simulated NPK sensor
  - Power: Battery level and solar charge rate
  - Location: GPS coordinates with field path tracking
- **Alert System**: Automatic detection of low nutrient levels with recommendations
- **Motor Control**: Directional commands for rover movement
- **Data Logging**: Time-stamped sensor readings including NPK values
- **CSV Export**: Download complete data logs for external analysis
- **REST API**:
  - `GET /` - Dashboard page
  - `GET /api/data` - Current rover state with NPK and alerts (JSON)
  - `GET /api/logs` - Recent sensor data logs (JSON)
  - `GET /api/export/csv` - Download data logs as CSV file

### Frontend (index.html)
- **Industrial Mission Control UI**: Dark theme with glassmorphism effects
- **Responsive Grid Layout**: 3-column (laptop) → 2-column (tablet) → 1-column (mobile)
- **GPS Field Mapping**: Leaflet map with rover position and field coverage path
- **Environmental Sensor Gauges**: Animated SVG semicircular gauges for pH, Moisture, Temperature
- **NPK Analysis Widget**: Vertical bar charts for Nitrogen, Phosphorus, Potassium (mg/kg)
- **Alert System**: Real-time popup notifications for low nutrient levels with fertilizer recommendations
- **Power Status**: Battery percentage and solar charge rate with progress bars
- **Interactive Controls**: 
  - Directional pad with mouse click or arrow keys (↑↓←→)
  - Spacebar for emergency stop
  - Red E-STOP button for immediate shutdown
- **Live Data Table**: Real-time log of recent readings (Time, pH, Moisture, Temp, N, P, K)
- **CSV Export Button**: Download entire data log for analysis in Excel/Google Sheets
- **WebSocket Client**: Receives live updates without page refresh

### Telemetry Data

Each 2-second sensor update includes:

**Environmental Sensors:**
- **Soil pH**: Range 6.0-8.5 (acidic to alkaline soil)
- **Soil Moisture**: Range 30-95% (dry to saturated soil)
- **Ambient Temperature**: 0-35°C dynamic range

**Soil Nutrient Analysis (NPK):**
- **Nitrogen (N)**: Range 0-400 mg/kg (critical for foliage growth)
- **Phosphorus (P)**: Range 0-80 mg/kg (critical for root development)
- **Potassium (K)**: Range 0-400 mg/kg (critical for plant strength)

**Power System:**
- **Battery Level**: 0-100% with real-time charge/discharge tracking
- **Solar Charge Rate**: 0-15W with efficiency tracking

**Location & Navigation:**
- **GPS Coordinates**: Latitude/longitude with ±0.0002 precision
- **Field Path**: Complete journey history (last 50 waypoints)

**Alert System:**
Automatic alerts trigger when:
- N < 100 mg/kg → "Low Nitrogen - Fertilizer Recommended"
- P < 20 mg/kg → "Low Phosphorus - Apply Phosphate Fertilizer"
- K < 100 mg/kg → "Low Potassium - Apply Potash Fertilizer"

## Controls

### Motor Control
- **↑ Forward** / **Arrow Up** - Drive forward
- **↓ Backward** / **Arrow Down** - Drive backward
- **← Left** / **Arrow Left** - Turn left
- **→ Right** / **Arrow Right** - Turn right
- **STOP** / **Spacebar** - Stop all motors
- **⚠ Emergency Stop** - Immediate shutdown

## Hardware Integration (Optional - NPK Sensor Support)

The Agrirover backend is designed to work with real NPK sensors using the **Modbus RS485 protocol**.

### Setting Up Real NPK Sensor:

1. **Hardware Requirements:**
   - USB-to-RS485 converter (plugged into Raspberry Pi)
   - RS485 NPK sensor (e.g., JXCT Soil Nutrient Sensor, DZN Soil EC Sensor)
   - 2-core shielded cable for RS485 connection

2. **Raspberry Pi Serial Setup:**
   ```bash
   # Enable serial port on Raspberry Pi
   sudo raspi-config
   # Navigate to Interface Options → Serial Port → Enable
   ```

3. **Install Modbus Library:**
   ```bash
   pip install pymodbus
   ```

4. **Modify app.py** - Replace sensor simulation with real NPK reading:
   ```python
   from pymodbus.client import ModbusSerialClient as ModbusClient
   
   # Initialize Modbus client
   client = ModbusClient(method='rtu', port='/dev/ttyUSB0', baudrate=9600)
   
   def read_npk_sensor():
       """Read real NPK values from Modbus sensor"""
       if client.connect():
           # Read holding registers for N, P, K
           result = client.read_holding_registers(0, 3, slave=1)
           return {
               'nitrogen': result.registers[0],
               'phosphorus': result.registers[1],
               'potassium': result.registers[2],
           }
   ```

5. **Other Sensors:**
   - **pH Sensor**: ADC via I2C (e.g., Atlas Scientific)
   - **Moisture Sensor**: Capacitive sensor via ADC
   - **Temperature**: DS18B20 1-Wire sensor
   - **Battery Voltage**: ADS1115 ADC
   - **Solar Panel Output**: INA219 power monitor

## API Endpoints

### GET /api/data
Returns current rover state with NPK analysis and alerts:
```json
{
  "status": "Online",
  "gps": {
    "latitude": 40.7128,
    "longitude": -74.0060,
    "path": [...]
  },
  "telemetry": {
    "soil_ph": 7.2,
    "soil_moisture": 65.0,
    "ambient_temp": 22.5,
    "battery_level": 85.0,
    "solar_charge_rate": 12.5
  },
  "npk": {
    "nitrogen": 180.5,
    "phosphorus": 35.2,
    "potassium": 220.1,
    "thresholds": {
      "nitrogen": {"low": 100, "optimal": [150, 300], "unit": "mg/kg"},
      "phosphorus": {"low": 20, "optimal": [25, 50], "unit": "mg/kg"},
      "potassium": {"low": 100, "optimal": [150, 300], "unit": "mg/kg"}
    }
  },
  "alerts": [
    {
      "nutrient": "Nitrogen",
      "level": 45.0,
      "threshold": 100,
      "recommendation": "Low Nitrogen - Fertilizer Recommended"
    }
  ]
}
```

### GET /api/logs
Returns last 20 sensor log entries with NPK values:
```json
[
  {
    "timestamp": "2026-01-26T14:30:45.123456",
    "soil_ph": 7.2,
    "soil_moisture": 65.0,
    "ambient_temp": 22.5,
    "battery_level": 85.0,
    "nitrogen": 180.5,
    "phosphorus": 35.2,
    "potassium": 220.1
  }
]
```

### GET /api/export/csv
Downloads complete data log as CSV file for analysis:
```
timestamp,soil_ph,soil_moisture,ambient_temp,battery_level,nitrogen,phosphorus,potassium
2026-01-26T14:30:45,7.2,65.0,22.5,85.0,180.5,35.2,220.1
2026-01-26T14:30:47,7.21,64.8,22.4,85.1,181.2,35.1,219.8
...
```

## Socket.io Events

### Client → Server
- `motor_command`: `{direction: 'forward'|'backward'|'left'|'right'|'stop'}`
- `emergency_stop`: Triggers immediate shutdown and alert

### Server → Client
- `telemetry_update`: Real-time sensor data including NPK values and alerts
- `motor_status`: Current motor power levels
- `emergency_stop_response`: Confirmation of emergency stop

## Performance Optimization

- **Low Power**: Minimal CPU usage, efficient WebSocket communication
- **Data Throttling**: Sensor updates every 2 seconds
- **Log Limiting**: Only last 100 entries kept in memory
- **Efficient Rendering**: CSS animations instead of JavaScript where possible

## Troubleshooting

**Port 5000 already in use?**
```bash
python app.py --port 5001
```

**WebSocket connection failed?**
- Check firewall settings
- Ensure CORS is enabled (should be by default)

**Gauge not updating?**
- Check browser console for WebSocket errors
- Verify socket.io is loaded: `console.log(io)`

## Tech Stack

- **Backend**: Python 3.8+, Flask, Flask-SocketIO
- **Frontend**: HTML5, CSS3, JavaScript (ES6+)
- **Mapping**: Leaflet.js, OpenStreetMap
- **Real-time**: Socket.io
- **Styling**: Custom CSS with glassmorphism

## Future Enhancements

- [ ] Real Modbus RS485 NPK sensor integration
- [ ] Database integration (SQLite/PostgreSQL) for historical data
- [ ] Advanced data analytics dashboard (Chart.js/Plotly)
- [ ] User authentication and multi-device support
- [ ] AI-based fertilizer recommendations using historical patterns
- [ ] Multi-rover management and fleet tracking
- [ ] Mobile app (React Native/Flutter)
- [ ] Predictive maintenance alerts
- [ ] Weather API integration for context
- [ ] Automated irrigation control system
- [ ] Cloud sync (AWS/Azure) for backup and analysis

## License

MIT

## Author

Agrirover Dashboard Development Team - 2026
