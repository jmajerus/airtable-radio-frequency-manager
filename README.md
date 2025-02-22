# Airtable Radio Frequency Manager

## Overview

Airtable Radio Frequency Manager is a **flexible, automated solution** for managing and exporting radio frequency data from **Airtable** to multiple formats compatible with hardware radios and SDR (Software-Defined Radio) applications.

## Features

✅ **Centralized Frequency Management** – Store all frequency data in Airtable for easy access and organization.\
✅ **Automatic Offset, Tone, and Step Size Calculations** – Uses band plans to pre-fill important radio settings.\
✅ **Multi-Format Export Support** – Converts Airtable data into formats for major SDR software and radio programming tools.\
✅ **CHIRP Radio Compatibility** – Supports CHIRP's latest radio database and custom radio configurations.\
✅ **Digital Mode Filtering** – Filter CHIRP-supported radios by common digital modes like **DMR, YSF, D-STAR, P25, NXDN, and TETRA**.\
✅ **Customizable Configuration** – Modify settings via `config.yaml` to match your workflow.\
✅ **Extensible Architecture** – Easily add new SDR software formats as needed.

## Supported Formats

This tool exports frequency data into formats compatible with:

- **CHIRP** (CSV for radio memory programming)
- **GQRX** (CSV for SDR bookmarks)
- **SDR# (SDRSharp)** (XML for SDR# frequency presets)
- **CubicSDR** (JSON/XML)
- **SDR++** (JSON)
- **SDRangel** (JSON)
- **HDSDR** (CSV/TXT)
- **SDRuno** (CSV)

## Airtable Structure

### **1️⃣ Frequencies Table (Main Table)**

Stores only **individual frequency records**, with **no band-specific or radio-specific fields**.

| Frequency (MHz) | Name           | Mode | Notes             | Band       |
| --------------- | -------------- | ---- | ----------------- | ---------- |
| 156.800         | Channel 16     | FM   | Distress & Safety | Marine VHF |
| 462.550         | GMRS Channel 1 | FM   | Repeater Input    | GMRS       |
| 26.965          | CB Channel 1   | AM   | General           | CB Radio   |

### **2️⃣ Band Table (New)**

Holds **all band-wide properties**.

| Band Name  | Frequency Range | Source      | Country | Power Limit (W) |
| ---------- | --------------- | ----------- | ------- | --------------- |
| Ham VHF    | 144-148 MHz     | IARU R2     | Global  | Varies          |
| GMRS       | 462-467 MHz     | FCC Part 95 | USA     | 50              |
| Marine VHF | 156-162 MHz     | ITU RR      | Global  | 25              |

### **3️⃣ Radio Table (New - CHIRP Support)**

Holds **CHIRP radio model-specific settings**.

| Radio Model   | CHIRP Support | Custom Step Size (kHz) | Power Setting | Notes                     |
| ------------- | ------------- | ---------------------- | ------------- | ------------------------- |
| Baofeng UV-5R | ✅ Built-in    | 12.5                   | High/Low      | N/A                       |
| Yaesu FT-60   | ✅ Built-in    | 5, 10, 12.5            | High/Low      | Needs manual offset input |
| Icom IC-7300  | ✅ Built-in    | Variable               | N/A           | HF Only                   |

## Installation

### Prerequisites

- **Python 3.x** installed on your system
- An **Airtable API Key** (available from your Airtable account)
- Install dependencies:
  ```sh
  pip install -r requirements.txt
  ```

## Configuration

Modify the `config/config.yaml` file to set up your **Airtable API key**, **base ID**, and **table names**. Example:

```yaml
api_key: "your_airtable_api_key"
base_id: "your_airtable_base_id"
table_name: "Frequencies"
export_formats:
  - chirp
  - gqrx
  - sdrsharp
  - sdrpp
  - cubicsdr
  - sdrangel
  - hdsdr
  - sdruno
```

## Usage

### **Export Frequency Data**

Run the script to sync data from Airtable and export it in multiple formats:

```sh
python cli.py
```

Or specify formats manually:

```sh
python cli.py --export chirp gqrx sdrsharp
```

### **List CHIRP-Supported Radios**

To fetch and display the latest **CHIRP-compatible radios**, run:

```sh
python cli.py --list-chirp-radios
```

To filter by brand:

```sh
python cli.py --list-chirp-radios --filter Baofeng
```

To filter by **digital mode**:

```sh
python cli.py --list-chirp-radios --digital DMR
```

To see available digital modes:

```sh
python cli.py --list-digital-modes
```

### **Import Standard Channel Lists**

To import predefined channel lists for CB, GMRS, and Marine VHF into Airtable:

```sh
python cli.py --import-standard CB_Radio GMRS Marine_VHF
```

## Contributing

Contributions are welcome! Feel free to submit a pull request or open an issue.

## License

This project is licensed under the MIT License.

