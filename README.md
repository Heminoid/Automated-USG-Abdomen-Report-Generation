# Automated USG Abdomen Report Generation

This project automates the generation of USG Abdomen reports (Ultrasonography Abdomen) from audio recordings. It leverages the power of Deepgram for speech-to-text conversion, OpenAI's GPT-4 for intelligent information extraction, and python-docx for creating formatted reports.

## Features

- **Audio Transcription**: Convert medical dictations to text using Deepgram's Nova-3 Medical model
- **Intelligent Information Extraction**: Process transcriptions with GPT-4 to extract structured medical data
- **Automated Report Generation**: Create formatted Word documents containing comprehensive USG findings
- **Medical Terminology Support**: Specialized handling of anatomical terms and measurements
- **User-Friendly Workflow**: Simple audio upload to formatted report download pipeline

## Workflow

```
Audio Recording → Deepgram Transcription → GPT-4 Information Extraction → Formatted Report Generation
```

1. **Audio Upload**
   - System accepts audio recordings of USG findings (WAV, MP3 formats)
   - Supports dictations from medical professionals

2. **Transcription**
   - Uses Deepgram's Nova-3 Medical model
   - Optimized for medical terminology and acoustic environments
   - Produces detailed text transcription with high accuracy

3. **Information Extraction**
   - Processes transcription using OpenAI's GPT-4
   - Custom system prompts extract structured medical information
   - Organizes data into JSON format with sections for:
     - Patient demographics
     - Organ measurements and observations
     - Pathological findings
     - Overall impression

4. **Report Generation**
   - Utilizes python-docx for document creation
   - Populates predefined templates with extracted information
   - Creates sections for each abdominal organ:
     - Liver
     - Gallbladder
     - Pancreas
     - Spleen
     - Kidneys
     - Other findings
   - Formats measurements and observations in clinical standard

5. **Report Download**
   - Automatically downloads the completed report as a .docx file
   - Ready for review, editing, or inclusion in medical records

## Technology Stack

- **Python**: Core programming language
- **Deepgram API**: Speech-to-text conversion with medical model
- **OpenAI GPT-4**: Natural language processing and information extraction
- **python-docx**: Document generation and formatting
- **JSON**: Structured data interchange

## Requirements

- Python 3.7 or higher
- deepgram-sdk==2.12.0
- openai
- python-docx
- Internet connection for API access

## Installation

```bash
# Clone the repository
git clone https://github.com/Heminoid/Automated-USG-Abdomen-Report-Generation
cd automated-usg-report

# Install dependencies
pip install -r requirements.txt

# Configure API keys
# (Edit config.py or use environment variables as described below)
```

## Configuration

The project requires API keys for Deepgram and OpenAI services:

1. **Deepgram API Key**:
   - Register at [console.deepgram.com](https://console.deepgram.com)
   - Create a new API key with Speech Recognition permissions
   - Set as environment variable: `export DEEPGRAM_API_KEY="your_key_here"`
   - Or edit directly in the code (not recommended for production)

2. **OpenAI API Key**:
   - Sign up at [platform.openai.com](https://platform.openai.com)
   - Create a new API key with GPT-4 access
   - Set as environment variable: `export OPENAI_API_KEY="your_key_here"`
   - Or initialize the client with: `client = OpenAI(api_key="your_key_here")`

## Usage

### Method 1: Google Colab

The easiest way to run this project is through Google Colab:

1. Open the provided Colab notebook
2. Run the setup cells to install dependencies
3. Enter your API keys when prompted
4. Upload your audio file using the file uploader widget
5. Execute the processing cells
6. Download the generated report

### Method 2: Local Execution

To run the project locally:

```python
from usg_report_generator import USGReportGenerator

# Initialize the generator with API keys
generator = USGReportGenerator(
    deepgram_api_key="your_deepgram_key",
    openai_api_key="your_openai_key"
)

# Process an audio file
report_path = generator.process_audio("path/to/audio_file.mp3")
print(f"Report generated at: {report_path}")
```


## How It Works

### Detailed Workflow

1. **Audio Transcription Process**:
   - Audio is processed in chunks optimal for medical dictations
   - Deepgram's Nova-3 Medical model applies specialized acoustic models
   - Returns timestamped text with medical term recognition

2. **Information Extraction with GPT-4**:
   - System prompt provides context about USG reports
   - Instructs the model to extract specific medical entities
   - Custom output structure ensures consistent format
   - Example prompt:
     ```
     You are a medical report assistant. Extract structured information from 
     the following ultrasound dictation, including measurements and findings 
     for all abdominal organs. Format the output as JSON with specific fields 
     for each organ system and overall impression.
     ```

3. **Report Generation Details**:
   - Uses professional medical templates
   - Structures information hierarchically by organ system
   - Maintains medical formatting standards for measurements
   - Includes sections for normal and abnormal findings

## API References

### Deepgram API

This project uses Deepgram's speech recognition API with the Nova-3 Medical model:

```python
from deepgram import DeepgramClient

# Initialize client
deepgram = DeepgramClient("YOUR_DEEPGRAM_API_KEY")

# Configure options for medical transcription
options = {
    "model": "nova-3-medical",
    "smart_format": True,
    "diarize": False,
    "language": "en-US"
}

# Process audio
response = deepgram.listen.prerecorded.v("1").transcribe_file(
    {"buffer": audio_data},
    options
)
```

### OpenAI API

The project leverages OpenAI's GPT-4 model for medical information extraction:

```python
from openai import OpenAI

client = OpenAI(api_key="YOUR_OPENAI_API_KEY")

response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "system", "content": "You are a medical assistant specializing in ultrasound reports."},
        {"role": "user", "content": f"Extract structured information from this USG transcription: {transcription}"}
    ],
    response_format={"type": "json_object"}
)
```

## Troubleshooting

### Common Issues

1. **Transcription Accuracy Issues**:
   - Ensure audio is clear with minimal background noise
   - Try preprocessing audio to enhance voice clarity
   - Consider recording in environments with minimal echo

2. **Information Extraction Problems**:
   - Review system prompts for clarity and specificity
   - Ensure transcription includes all necessary medical details
   - Adjust temperature and other GPT-4 parameters for more deterministic outputs

3. **Report Formatting Issues**:
   - Check template formatting and placeholders
   - Ensure JSON output structure matches template expectations
   - Verify python-docx version compatibility

## Contributing

Contributions to improve the project are welcome! Here's how you can contribute:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add some amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Development Guidelines

- Follow PEP 8 style guidelines for Python code
- Add unit tests for new features
- Update documentation to reflect changes
- Include example outputs where appropriate

## Disclaimer

This project is intended for educational and demonstration purposes only. It is not a replacement for professional medical services or advice. The generated reports should always be reviewed and approved by qualified healthcare professionals before use in clinical settings.

The accuracy of the reports depends on the quality of the input audio and the capabilities of the underlying AI models. Users should verify all information in the generated reports.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contact & Support

For questions, feature requests, or support:
- **GitHub Issues**: Open an issue in this repository
