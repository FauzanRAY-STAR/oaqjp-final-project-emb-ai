# Final Project

Emotion detector web application for the IBM/Coursera Python and Flask project. It calls the course Watson NLP emotion service, formats five emotion scores, packages the detector as `EmotionDetection`, includes unit tests, and exposes a Flask web interface.

## Setup

Run inside the course Skills Network environment if you need to access the lab's Watson endpoint:

```bash
cd /home/project/final_project
python3 -m pip install -r requirements.txt
```

## Run the detector

```bash
python3 -c "from EmotionDetection.emotion_detection import emotion_detector; print(emotion_detector('I am really mad about this'))"
```

The service's numerical scores can vary with the text and service response. A valid response contains `anger`, `disgust`, `fear`, `joy`, `sadness`, and `dominant_emotion`.

## Run unit tests and static analysis

```bash
python3 test_emotion_detection.py
pylint server.py
```

The unit tests mock the network request, so they verify application logic without depending on live Watson service availability.

## Run the web application

```bash
python3 server.py
```

Open `http://127.0.0.1:5000`. The `EmotionDetection` API package is imported by `server.py`. Empty browser input is rejected before making an API call, and the API endpoint returns HTTP 400 for empty input.

## GitHub submission

Fork the original repository `https://github.com/ibm-developer-skills-network/oaqjp-final-project-emb-ai` and name your fork `oaqjp-final-project-emb-ai`. Ensure the README starts with `# Final Project`, then upload/commit your completed files. Replace `<USERNAME>` in the URLs below with your GitHub username:

- `https://github.com/<USERNAME>/oaqjp-final-project-emb-ai/blob/main/README.md`
- `https://github.com/<USERNAME>/oaqjp-final-project-emb-ai/blob/main/EmotionDetection/__init__.py`

Capture screenshots from your own running application for the two image-upload questions. Do not submit terminal output or screenshots unless you have actually generated them in your environment.
