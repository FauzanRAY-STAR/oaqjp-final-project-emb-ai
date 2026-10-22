"""Detect five emotions in text through the Watson NLP course service."""

from typing import Any

import requests

WATSON_EMOTION_URL = (
    "https://sn-watson-emotion.labs.skills.network/"
    "v1/watson.runtime.nlp.v1/NlpService/EmotionPredict"
)
MODEL_ID = "emotion_aggregated-workflow_lang_en_stock"
EMOTION_NAMES = ("anger", "disgust", "fear", "joy", "sadness")


def _empty_result() -> dict[str, Any]:
    """Return the standard response when input or service data is invalid."""
    result = {emotion: None for emotion in EMOTION_NAMES}
    result["dominant_emotion"] = None
    return result


def emotion_detector(text_to_analyse: str) -> dict[str, Any]:
    """Return scores for anger, disgust, fear, joy, sadness and the dominant one.

    Empty input, HTTP errors, service errors, and malformed responses return a
    dictionary whose emotion values and dominant_emotion are all None.
    """
    if not isinstance(text_to_analyse, str) or not text_to_analyse.strip():
        return _empty_result()

    headers = {"grpc-metadata-mm-model-id": MODEL_ID}
    payload = {"raw_document": {"text": text_to_analyse}}

    try:
        response = requests.post(
            WATSON_EMOTION_URL,
            json=payload,
            headers=headers,
            timeout=30,
        )
    except requests.RequestException:
        return _empty_result()

    if response.status_code != 200:
        return _empty_result()

    try:
        predictions = response.json()["emotionPredictions"]
        emotion_scores = predictions[0]["emotion"]
        scores = {emotion: float(emotion_scores[emotion]) for emotion in EMOTION_NAMES}
    except (KeyError, IndexError, TypeError, ValueError):
        return _empty_result()

    result = dict(scores)
    result["dominant_emotion"] = max(scores, key=scores.get)
    return result
