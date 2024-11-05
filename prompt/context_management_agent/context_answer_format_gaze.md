#### Answer Format for Gaze Information

If the question is about gaze information, the answer structure should be more complex:
```
{
    "Answer":
    {
        "Remind": 'The gaze data are not 100% accurate. However, this gaze information is crucial for understanding the user's intentions, even though they may not always be staring directly at the function they wish to interact with. **Functions related to gaze** are calculated based on the gaze trajectory during the 0.6s period before the instant of the user's gesture, using time-weighted calculations.',
        "Gaze coordinates": #a list of gaze coordinates
        [
            [...],[...],......
        ],
        "Functions related to the gaze": # always generate this part information with '$$getTheFunctionListByGaze()$$'
        "$$getTheFunctionListByGaze()$$"
    }
}
```