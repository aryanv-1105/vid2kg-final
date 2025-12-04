vid2kg
A multilingual recipe extraction pipeline that converts Indian cooking videos from YouTube into structured, machine-readable JSON format.
Description
vid2kg (Video to Knowledge Graph) is an automated system designed to extract structured recipe information from YouTube cooking videos, with a focus on multilingual Indian content. The pipeline processes videos through six stages: audio extraction, speech-to-text transcription, translation, error correction, LLM-based extraction, and unit normalization. It handles videos in Hindi, Tamil, Telugu, Bengali, Malayalam, and English, dealing with challenges like regional accents, code-mixing (Hinglish/Tanglish), and traditional Indian measurement units (katori, chamach). The system achieves 85% overall accuracy and outputs clean JSON files ready for integration with Food Knowledge Graphs.
Getting Started
Dependencies

Operating System: macOS, Linux, or Windows with WSL2
Python: Version 3.9 or higher
RAM: Minimum 8GB (16GB recommended)
Storage: 5GB free space for models and dependencies
Internet Connection: Required for YouTube downloads and API calls
Google Gemini API Key: Required for LLM-based recipe extraction (free tier available at https://makersuite.google.com/app/apikey)

Key Python Libraries:

yt-dlp - YouTube audio download
faster-whisper or openai-whisper - Speech-to-text conversion
googletrans==4.0.0-rc1 - Translation
langid - Language detection
rapidfuzz - Fuzzy string matching
google-generativeai - Gemini API client
rich - Terminal UI with progress tracking

Installing

Clone the repository:

git clone https://github.com/aryanv-1105/vid2kg-final.git
cd vid2kg-final

Create and activate a virtual environment:

python3 -m venv venv
source venv/bin/activate  # On macOS/Linux
venv\Scripts\activate     # On Windows

Install required packages:

pip install -r requirements.txt

Download the Whisper model (happens automatically on first run, or pre-download):

python -c "from faster_whisper import WhisperModel; WhisperModel('small')"

Set up your API key:

export GEMINI_API_KEY="your-api-key-here"
Executing program

Basic usage - process a YouTube video:

python -m vid2kg.cli --url "https://www.youtube.com/watch?v=VIDEO_ID" --out data/output

Complete example with all options:

python -m vid2kg.cli \
  --url "https://www.youtube.com/watch?v=7QKOPdBg5co" \
  --out data/dal_fry_recipe \
  --model small \
  --engine faster \
  --verbose

Process a local audio file instead of YouTube URL:

python -m vid2kg.cli --input /path/to/audio.m4a --out data/output --model small

Skip specific pipeline stages (for debugging or custom workflows):

python -m vid2kg.cli \
  --url "YOUTUBE_URL" \
  --out data/output \
  --skip-translation \
  --skip-fuzzy \
  --skip-llm
Available Command-Line Arguments:

--url - YouTube video URL
--input - Local audio file path (alternative to --url)
--out - Output directory (required)
--model - Whisper model size: tiny, small (default), medium, large
--engine - ASR engine: faster (default) or whisper
--skip-translation - Skip translation stage
--skip-fuzzy - Skip error correction
--skip-llm - Use regex parser instead of LLM
--skip-normalization - Skip unit conversion
--force-translate - Force translation even if English detected
--verbose - Enable detailed logging

Help
Common issues and solutions:
YouTube download fails:
# Update yt-dlp to latest version
pip install --upgrade yt-dlp

# Note: YouTube blocks yt-dlp from AWS/cloud environments - run locally
Whisper model not found:
# Manually download the model
python -c "from faster_whisper import WhisperModel; WhisperModel('small')"
Gemini API rate limit (429 error):
# Wait 60 seconds and retry
# Check quota at https://makersuite.google.com
# Process videos sequentially, not in parallel
Out of memory error:
# Use smaller model
python -m vid2kg.cli --url "URL" --out data/output --model tiny

# Or close other applications to free up RAM
Poor transcription quality:
# Try larger model for better accuracy
python -m vid2kg.cli --url "URL" --out data/output --model medium

# Use --verbose flag to inspect raw transcription
python -m vid2kg.cli --url "URL" --out data/output --verbose
Authors
Aryan Verma - @aryanv-1105
Avik Mittal - @avikmittal
Ashoka University Capstone Project - December 2025
Version History

1.0

Initial Release
Six-stage pipeline: audio extraction, transcription, translation, error correction, LLM extraction, normalization
Support for 5+ Indian languages (Hindi, Tamil, Telugu, Bengali, Malayalam)
85% overall accuracy, 90% ingredient extraction accuracy
Fuzzy matching error correction (60-80% error reduction)
Google Gemini 2.5 Flash integration
Rich terminal UI with progress tracking



License
This project is licensed under the MIT License - see the LICENSE.md file for details
Acknowledgments
Inspiration, code snippets, and tools used:

OpenAI Whisper - Speech recognition model
yt-dlp - YouTube download tool
Google Gemini API - LLM for recipe extraction
RapidFuzz - Fuzzy string matching
Rich - Terminal formatting
