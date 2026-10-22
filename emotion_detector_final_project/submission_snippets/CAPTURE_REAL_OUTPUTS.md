# Capture real command outputs for the submission

The Watson NLP service must be called from the course environment. Its real scores and network response cannot be honestly recreated offline. Run these commands in the Skills Network terminal and copy the complete output shown there into the matching Coursera response fields.

## Task 2 / Question 3: import and smoke test

```bash
cd /home/project/final_project
python3
```

Then at the Python prompt:

```python
from emotion_detection import emotion_detector
print(emotion_detector('I am really mad about this'))
exit()
```

Then:

```bash
python3 test_emotion_detection.py
```

Copy the complete terminal interaction, ensuring the prompt/path shows `/home/project/final_project` as requested by the assignment.

## Task 3 / Question 5: formatted response

```bash
cd /home/project/final_project
python3 -c "from emotion_detection import emotion_detector; print(emotion_detector('I am really mad about this'))"
```

Copy the response dictionary containing `anger`, `disgust`, `fear`, `joy`, `sadness`, and `dominant_emotion`.

## Task 4 / Question 7: package import

```bash
cd /home/project/final_project
python3 -c "from EmotionDetection.emotion_detection import emotion_detector; print(emotion_detector('I am very angry'))"
```

Copy the actual output. Check that the dictionary has all five emotion scores and `dominant_emotion`. Do not manually change the output to say `anger` if the actual API response differs.

## Task 5 / Question 9: unit tests

```bash
cd /home/project/final_project
python3 test_emotion_detection.py
```

## Task 8 / Question 16: Pylint

```bash
cd /home/project/final_project
pylint server.py
```

Copy the actual Pylint output including the final score. Scores depend on the exact file and installed Pylint version.

## Screenshot uploads (Questions 11 and 14)

Run `python3 server.py`, open the displayed local web address in a browser, analyze a non-empty English sentence, and take a screenshot of the working interface for Question 11. For Question 14, submit a blank input and screenshot the validation message. Upload screenshots you actually take from your own running app; do not use fabricated screenshots.
