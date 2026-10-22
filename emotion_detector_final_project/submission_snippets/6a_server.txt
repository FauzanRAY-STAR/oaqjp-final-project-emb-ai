"""Flask web application for the Watson NLP emotion detector."""

from flask import Flask, jsonify, render_template, request

from EmotionDetection.emotion_detection import emotion_detector

app = Flask(__name__)


@app.get("/")
def index():
    """Render the main emotion detector interface."""
    return render_template("index.html")


@app.get("/emotionDetector")
def emotion_detector_route():
    """Analyze the text query parameter and return formatted emotion data."""
    text_to_analyse = request.args.get("textToAnalyze", "")
    if not text_to_analyse.strip():
        return (
            "Invalid input! Please enter text to analyze.",
            400,
            {"Content-Type": "text/plain; charset=utf-8"},
        )

    result = emotion_detector(text_to_analyse)
    if result["dominant_emotion"] is None:
        return (
            "Emotion analysis service is unavailable or returned invalid data.",
            502,
            {"Content-Type": "text/plain; charset=utf-8"},
        )

    if request.args.get("format") == "json":
        return jsonify(result)

    scores = ", ".join(
        f"'{emotion}': {result[emotion]}"
        for emotion in ("anger", "disgust", "fear", "joy", "sadness")
    )
    return (
        f"For the given statement, the system response is {scores}. "
        f"The dominant emotion is {result['dominant_emotion']}."
    )


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=False)
