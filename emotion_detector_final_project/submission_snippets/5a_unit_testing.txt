"""Unit tests for the Watson-backed emotion detector."""

import unittest
from unittest.mock import Mock, patch

import requests

from EmotionDetection.emotion_detection import (
    EMOTION_NAMES,
    WATSON_EMOTION_URL,
    emotion_detector,
)


class EmotionDetectorTests(unittest.TestCase):
    """Test successful predictions and safe handling of invalid responses."""

    def make_response(self, scores, status_code=200):
        """Create a mock response in the format returned by Watson NLP."""
        response = Mock()
        response.status_code = status_code
        response.json.return_value = {
            "emotionPredictions": [{"emotion": scores}]
        }
        return response

    @patch("EmotionDetection.emotion_detection.requests.post")
    def test_successful_prediction(self, post_mock):
        scores = {
            "anger": 0.10,
            "disgust": 0.05,
            "fear": 0.10,
            "joy": 0.70,
            "sadness": 0.05,
        }
        post_mock.return_value = self.make_response(scores)

        result = emotion_detector("I am feeling great today")

        self.assertEqual(result["dominant_emotion"], "joy")
        for emotion in EMOTION_NAMES:
            self.assertEqual(result[emotion], scores[emotion])
        post_mock.assert_called_once_with(
            WATSON_EMOTION_URL,
            json={"raw_document": {"text": "I am feeling great today"}},
            headers={
                "grpc-metadata-mm-model-id":
                "emotion_aggregated-workflow_lang_en_stock"
            },
            timeout=30,
        )

    @patch("EmotionDetection.emotion_detection.requests.post")
    def test_http_400_returns_empty_result(self, post_mock):
        post_mock.return_value = self.make_response({}, status_code=400)
        result = emotion_detector("bad request")
        self.assertIsNone(result["dominant_emotion"])
        self.assertTrue(all(result[name] is None for name in EMOTION_NAMES))

    @patch("EmotionDetection.emotion_detection.requests.post")
    def test_blank_input_does_not_call_service(self, post_mock):
        result = emotion_detector("   ")
        post_mock.assert_not_called()
        self.assertIsNone(result["dominant_emotion"])

    @patch("EmotionDetection.emotion_detection.requests.post")
    def test_request_error_returns_empty_result(self, post_mock):
        post_mock.side_effect = requests.RequestException("service unavailable")
        result = emotion_detector("hello")
        self.assertIsNone(result["dominant_emotion"])

    @patch("EmotionDetection.emotion_detection.requests.post")
    def test_malformed_json_returns_empty_result(self, post_mock):
        response = Mock()
        response.status_code = 200
        response.json.return_value = {"unexpected": []}
        post_mock.return_value = response
        result = emotion_detector("hello")
        self.assertIsNone(result["dominant_emotion"])


if __name__ == "__main__":
    unittest.main()
