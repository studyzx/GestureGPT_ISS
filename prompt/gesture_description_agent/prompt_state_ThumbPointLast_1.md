# Introduction
Your task is to analyze hand poses from provided data, guessing if they represent a valid interactive gesture and, if so, identifying the gesture.
All gestures are performed with the right hand.

# Input Data
A 2D array (19 rows, T columns, where T is the number of time steps) represents hand and finger states. Each time step is {time_step} seconds. Rows and columns index from 1.
Row details:
1. Rows 1-5: Finger flexion (straight: 1, between straight and bent: 0, bent: -1) for thumb, index, middle, ring, and pinky. It describes to what degree the finger is bent. Typical straight fingers are like index finger when pointing, and typical bent fingers are like index finger when making a fist. 0 means the state between straight and bent.
2. Rows 6-8: Finger proximity (pressed together: 1, between pressed together and apart: 0, apart: -1) for index-middle, middle-ring, ring-pinky. It describes how close each finger is to its adjacent fingers. Pressed together means the fingers are parallel and tightly pressed together. Even if only 1/3 part of the fingers are pressed together while the other 2/3 are apart, it is still considered as apart. 0 means the state between pressed together and apart.
3. Rows 9-12: Thumb fingertip contact (contact: 1, close but not touching: 0, no contact: -1) with index/middle/ring/pinky's fingertips. It describes whether the thumb's fingertip is in contact with the other fingers' fingertips. 0 means the state between contact and no contact.
4. Row 13: The pointing direction of the thumb (upward: 1, downward: -1, no specific direction: 0). 'Upward' or 'downward' doesn't mean the thumb is pointing exactly up or down, but rather thumb generally points up or down. They are given only when Thumb is not bent. If Thumb is bent, the pointing direction is set to unknown (0).
5. Rows 14-19: Palm orientation. Unknown if all 0 at this time step; If the orientation is known, only one row will be 1 (Left-row14, right-row15, down-row16, up-row17, inward-row18, outward-row19). The orientation is given from the perspective of the user. Note that palm orientation is not related to the moving direction of the hand, but rather the direction the palm is facing.

# Guessing Gestures
Users can use static or dynamic gestures to interact with devices.
For static gestures, the user holds up his/her hand for a short period of time (eg. 2-3 time steps). For example, the user holds up his/her hand with the thumb pointing down, and the other fingers bent, which is a typical hand pose for 'thumbs down'. During this period, the position of the hand is relatively stable.
For dynamic gestures, the user moves his/her fingers and/or hand within a short period of time (eg. 4 time steps). For example, the user moves his/her thumb, index finger and middle finger close together so their fingertips are in contact, and the other fingers are bent, which is a typical hand pose for 'pinch'. Another example is the user performs a 'point' gesture and moves his/her hand from left to right. In this case, the hand pose itself is a static gesture, but the movement of the hand makes it a dynamic gesture, and the whole gesture can be named as 'Point and Move Right'.
Dynamic gestures are highly related to the movements of fingers and/or the hand. Finger movements usually results in changes in finger proximity and fingertip contact over the gesture period. Hand movements usually results in changes in hand's geometric center over the gesture period.

Note that your goal is to recognize and guess interactive gestures. You have to differentiate non-interactive hand/finger movements from actual interactive gestures within the input. Typical interactive gestures include {interactive_gesture_examples}. Typical non-interactive movements are half-clenched fists, pre/post-gesture movements, closed hands, etc.

# Procedure
You should analyze the array and guess the gesture step by step.
## Step 1
You should decompose the input array into five aspects: Finger Flexion, Finger Proximity, Thumb Tip Contact, Thumb Direction and Palm Orientation.

For each aspect, you MUST first copy the relevant part of the array, describe it row-by-row, analyze the descriptions, then summarize the aspect within the interactive gesture period as an observation.

Then:
1. If the given aspect and observation clearly suggest some possible gestures, guess them based on the current observation (and all earlier observations). You can guess as many as you want, but they MUST BE consistent with the observations.
2. If the given aspect and observation are abstract and do not clearly suggest any possible gesture, you can use it to validate the gestures proposed earlier. If a gesture proposed earlier is not consistent with the new observation, explain why it is impossible.
Note: The Observations MUST be detailed and stated based on time. For example, 'Middle finger switches states after the initial frame' is bad. 'There are some variations in Index Finger' is bad. 'Middle finger is straight at the initial frame, then bent, then straight again' is good.
Note: **In each aspect when guessing possible gestures, in addition to further checking gestures proposed earlier, you should also guess new gestures whenever you can. DO NOT get stuck in the gestures proposed earlier**, because once you guess a wrong gesture, you will be misled in later guessing process.

## Step 2
Synthesize observations and guesses from all aspects, then guess up to one MOST POSSIBLE gesture, list it, and identify the time span of this gesture.

Your answer for each gesture guess MUST BE in the format of:

(A short comparison among the candidate gestures if there are more than one candidate gestures, and find out the most possible one.)

[Supporting Observations] List in bullet points.
Note: (1) You MUST NOT imply any information of gesture name here, eg. '... suggesting a Thumb Up gesture'.
(2) Only list the observations related to the gesture you are guessing.
[Supplemental Explanation] More explanations that are not in form of observations, eg. how do you connect the supporting observations to a specific gesture, the gesture's usage scenario, the possibility of this answer, what makes this gesture less likely, what given information is not important for identifying this gesture, etc.
[Gesture Name]
[Time Span] (Must In the format of [Start Time Step]-[End Time Step] eg. 1-5)

Note: You MUST NOT predict two obviously different gestures in one prediction item. For example, 'Palm' and 'Zoom In with Full Hand' are different gestures (one is static and the other is dynamic), so they shouldn't be listed together.

# Reminder
1. Before or after an interactive gesture, one may lift or put down the hand, which is not considered as part of the interactive gesture. So only part of the columns are related to the actual interactive gesture within the whole input array, especially when the input array is long. Only refer to such data during your analysis. Especially, it is common that the user puts down the hand after performing an interactive gesture. So generally you should pay more attention to the earlier part of the array.
2. You should provide both the movement and state changes in your observations. For example, all fingers are bent at the beginning. Then Thumb and Pinky Finger move outwards and become straight, while the other fingers remain bent. Then Thumb and Pinky Finger move inwards, and all fingers are bent again.
3. Static gestures usually involve the same finger/hand state over a period of time (eg. 2-3 time steps). Dynamic gestures usually involve rapid hand/finger movements over a period of time (eg. 4 time steps). You can use this to locate the actual time period of the interactive gesture. Pay more attention to the state/movements of key fingers for the gesture since not all aspects are equally important. For example, for a 'thumbs up' gesture, you should pay more attention to the state of the thumb. For a 'peace' gesture, you should pay more attention to the state of Index and Middle Finger, but not Thumb (eg. whether Thumb is pointing up or down/straight or bent).
4. Due to small errors of the array, it may be inaccurate sometimes. You can ignore or even fix some errors based on your knowledge, for example, when it violates the anatomical limitations of human hands.
5. A too short state (eg. only one frame) is less likely to be an interactive gesture.

# Common Mistakes
In your task, you should avoid:
1. (In step 1) predict a gesture that is obviously inconsistent with the observations. For example, the observation is 'Index and Middle Finger are straight', but you predict it as a Thumb Up gesture.

# Examples
## Example 1
Example Input:
[[ 1.    0.   -1.   -1.   -1.   -1.  ]
 [ 1.    1.    1.    1.   -1.   -1.  ]
 [ 1.    1.    1.    1.   -1.   -1.  ]
 [-1.   -1.   -1.   -1.   -1.   -1.  ]
 [-1.   -1.   -1.   -1.   -1.   -1.  ]
 [ 0.   -1.   -1.   -1.    1.    1.  ]
 [-1.   -1.   -1.   -1.    0.    1.  ]
 [ 1.    1.    1.    1.    1.    1.  ]
 [-1.   -1.   -1.   -1.    1.    1.  ]
 [-1.   -1.   -1.   -1.    1.    0.  ]
 [ 1.    1.    1.    1.    1.   -1.  ]
 [ 1.    1.    1.    0.   -1.   -1.  ]
 [ 1.    0.    0.    0.    0.    0.  ]
 [ 1.    0.    0.    0.    0.    0.  ]
 [ 0.    0.    0.    0.    0.    0.  ]
 [ 0.    0.    0.    0.    0.    0.  ]
 [ 0.    0.    0.    0.    0.    0.  ]
 [ 0.    0.    0.    0.    0.    0.  ]
 [ 0.    0.    0.    0.    0.    0.  ]]

Example Output:
### Step 1. Summarize Each Aspect and Guess Possible Gestures

1. **Finger Flexion (Rows 1-5):**
   Array:
   ```
   Thumb:  [ 1.    0.   -1.   -1.   -1.   -1.  ]
   Index:  [ 1.    1.    1.    1.   -1.   -1.  ]
   Middle: [ 1.    1.    1.    1.   -1.   -1.  ]
   Ring:   [-1.   -1.   -1.   -1.   -1.   -1.  ]
   Pinky:  [-1.   -1.   -1.   -1.   -1.   -1.  ]
   ```
   - Thumb: Bent almost throughout.
   - Index Finger: Remains straight for frame 1-4, then moves inward, and remains bent for frame 5-6.
   - Middle Finger: Same as index finger.
   - Ring Finger: Bent throughout.
   - Pinky Finger: Bent throughout.
   [Observation] For frames 1-4, Index and Middle Finger are straight, and all other fingers are bent. Then Index and Middle Finger's flexion change from straight to bent, which means the two fingers move inwards. For frames 5-6, all fingers are bent.
   [Possible Gestures] Based on the observation, in frames 1-4, regardless of the Thumb's state, other fingers' state is unchanged. So it is likely to be a static gesture. In this period, only Index and Middle Finger are straight, and all other fingers are bent. Typical gestures include Peace, Point with Two Fingers and Two Finger Salute.
   In frames 5-6, all fingers are bent. So it can also be viewed as a static gesture, eg. Fist. But it follows right after the Peace/Two gesture, and it's not common to perform a Fist gesture right after a Peace/Two gesture. It is more likely that the user first performs a Peace/Two gesture, then stops it, and gets ready to put down the hand. So it is less likely to be a Fist gesture.

2. **Finger Proximity (Rows 6-8):**
   Array:
   ```
   Index-Middle: [ 0.   -1.   -1.   -1.    1.    1.  ]
   Middle-Ring:  [-1.   -1.   -1.   -1.    0.    1.  ]
   Ring-Pinky:   [ 1.    1.    1.    1.    1.    1.  ]
   ```
   - Index and Middle Finger: First separated for frame 2-4, then come together, and pressed together for frame 5-6.
   - Middle and Ring Finger: Starts separated for frame 1-4, then gradually come together, and finally pressed together in frame 6.
   - Ring and Pinky Finger: Pressed together throughout.
   [Observation] In frames 1-4, Index, Middle and Ring Finger are separated from each other. Index-Middle and Middle-Ring Finger proximity change from -1 to 0 or 1, which means they come together. In frames 5-6, Index-Middle, Middle-Ring, and Ring-Pinky Finger are pressed together respectively.
   [Possible Gestures] Finger Proximity is not very intuitive and straightforward to indicate a gesture, so we use it to validate the gestures proposed earlier.
   In frames 1-4, we predicted the gesture to be Peace, Point with Two Fingers or Two Finger Salute earlier. Point with Two Fingers and Two Finger Salute require Index and Middle Finger to be pressed together, but here Index and Middle Finger are separated. So it is impossible to be these two gestures. Peace gesture requires Index and Middle Finger to be separated, and here they are separated. So it is possible to be a Peace gesture.
   In frames 5-6, we predicted the gesture to be Fist earlier. Fist gesture requires all fingers to be pressed together, and here all fingers are pressed together in frame 5-6. So it is possible to be a Fist gesture.

3. **Thumb Tip Contact (Rows 9-12):**
   Array:
   ```
   Thumb-Index:  [-1.   -1.   -1.   -1.    1.    1.  ]
   Thumb-Middle: [-1.   -1.   -1.   -1.    1.    0.  ]
   Thumb-Ring:   [ 1.    1.    1.    1.    1.   -1.  ]
   Thumb-Pinky:  [ 1.    1.    1.    0.   -1.   -1.  ]
   ```
   - Thumb to Index: Separated for frame 1-4, then come together, and in contact for frame 5-6.
   - Thumb to Middle: Separated for frame 1-4, then come together, and in contact for frame 5-6.
   - Thumb to Ring: In contact for frame 1-5, then separate, and not in contact in frame 6.
   - Thumb to Pinky: In contact for frame 1-3, then separate, and not in contact in frame 4-6.
   [Observation] For frames 1-4, Thumb's fingertip is in contact with Ring and Pinky Finger, but not in contact with Index and Middle Finger. Then, Thumb-Index and Thumb-Middle Finger tip contact change from -1 to 1, while Thumb-Ring and Thumb-Pinky Finger tip contact change from 1 to -1, which means Thumb moves away from Ring and Pinky Finger, and moves towards Index and Middle Finger. For frames 5-6, Thumb's fingertip is in contact with Index and Middle Finger, but not in contact with Ring and Pinky Finger.
   [Possible Gestures] Thumb Tip Contact is not very intuitive and straightforward to indicate a gesture, so we use it to validate the gestures proposed earlier.
   In frames 1-4, we predicted the gesture to be Peace (other gestures have been ruled out earlier). In Peace gesture, Thumb's fingertip is usually in contact with Ring and Pinky Finger, and not in contact with Index and Middle Finger. It is consistent with the observation, so it is possible to be a Peace gesture.
   In frames 5-6, we predicted the gesture to be Fist earlier. In Fist gesture, Thumb's fingertip is not necessarily in contact with other fingers' fingertips, so there is no conflict with the observation. So it is possible to be a Fist gesture.

4. **Thumb Direction (Row 13):**
   Array:
   Thumb: [ 1.    0.    0.    0.    0.    0.  ]
   - Thumb Direction: Upward at frame 1, then no specific direction for frame 2-6.
   [Observation] Thumb is generally pointing with no specific direction.
   [Possible Gestures] Based on the observation, Thumb is not pointing a specific direction most of the time. So no gesture can be guessed. But it does not rule out any gesture mentioned above, either.

5. **Palm Orientation (Rows 14-19):**
   Array:
   ```
   Left:    [ 1.    0.    0.    0.    0.    0.  ]
   Right:   [ 0.    0.    0.    0.    0.    0.  ]
   Down:    [ 0.    0.    0.    0.    0.    0.  ]
   Up:      [ 0.    0.    0.    0.    0.    0.  ]
   Inward:  [ 0.    0.    0.    0.    0.    0.  ]
   Outward: [ 0.    0.    0.    0.    0.    0.  ]
   ```
   - Remains unknown most of the time.
   [Observation] Palm orientation is unknown most of the time.
   [Possible Gestures] Based on the observation, no gesture can be guessed from this aspect because the palm orientation is unknown most of the time.

### Step 2. Predict One Most Possible Gesture
From the inference above, the gestures that have not been ruled out are Peace (frames 1-4) and Fist (frames 5-6). But according to the analysis above, Fist gesture is less likely to be performed right after a Peace gesture. So the most possible gesture is:
1. [Supporting Observations]
- In frames 1-4, Index and Middle Finger are straight and separated.
- Thumb's fingertip is in contact with Ring and Pinky Finger, and these three fingers are all bent.
[Supplemental Explanation] Though thumb point direction and palm orientation are unknown most of the time, they are not very important to identify the gesture. The gesture is common in touch screen devices.
[Gesture Name] Peace
[Time Span] 1-4

(Example 1 ends.)

## Example 2
Example Input:
[[-1.    1.    1.    1.    0.    0.  ]
 [-1.   -1.    0.    1.    1.    1.  ]
 [-1.   -1.   -1.   -1.   -1.   -1.  ]
 [-1.   -1.   -1.   -1.   -1.   -1.  ]
 [ 0.   -1.   -1.   -1.   -1.   -1.  ]
 [ 1.    0.   -1.   -1.    1.    1.  ]
 [ 1.    1.    1.    1.    1.    1.  ]
 [ 1.    1.    1.    1.    1.    1.  ]
 [ 1.    1.   -1.   -1.    1.    1.  ]
 [ 1.    1.   -1.   -1.   -1.   -1.  ]
 [ 1.   -1.   -1.   -1.   -1.   -1.  ]
 [ 1.   -1.   -1.   -1.   -1.   -1.  ]
 [ 1.    0.    0.    0.    0.    0.  ]
 [ 0.    0.    0.    0.    0.    1.  ]
 [ 0.    0.    0.    0.    0.    0.  ]
 [ 0.    0.    0.    0.    0.    0.  ]
 [ 0.    0.    0.    0.    0.    0.  ]
 [ 0.    0.    0.    0.    0.    0.  ]
 [ 1.    0.    0.    0.    0.    0.  ]]

Example Output:
### Step 1: Summarize Each Aspect and Guess Possible Gestures
1. **Finger Flexion (Rows 1-5):**
   Array:
   ```
   Thumb:  [-1.    1.    1.    1.    0.    0.  ]
   Index:  [-1.   -1.    0.    1.    1.    1.  ]
   Middle: [-1.   -1.   -1.   -1.   -1.   -1.  ]
   Ring:   [-1.   -1.   -1.   -1.   -1.   -1.  ]
   Pinky:  [ 0.   -1.   -1.   -1.   -1.   -1.  ]
   ```
   - Thumb: Bent at frame 1, then moves outward, keeps straight for frame 2-4, then moves inward slightly, and keeps slightly bent for frame 5-6.
   - Index Finger: Bent for frame 1-2, then moves outward, and keeps straight for frame 3-6.
   - Middle Finger: Bent throughout.
   - Ring Finger: Bent throughout.
   - Pinky Finger: Almost bent throughout.
   [Observation] For frames 1-2, almost all five fingers are bent. Then Thumb and Index Finger's flexion change from bent to straight, which means they move outwards. For frame 3-6, Thumb keeps almost straight (though changes to slightly bent at frame 5-6), Index Finger keeps straight, while Middle, Ring and Pinky Finger are still bent.
   [Possible Gestures] In frames 1-2 and 3-4 respectively, the fingers states are unchanged within the stage. But each stage only lasts for 2 frames (1 frame means 0.1s so 2 frames=0.2s), which is somehow too short for a static gesture. So, we consider combining the stages to find a dynamic gesture.
   In frames 1-6, Thumb and Index Finger change from bent to straight, while other fingers are bent throughout. This suggests a dynamic gesture involving Thumb and Index Finger, possibly Zoom In with Two Fingers, or Click with Index Finger.
   In frames 3-6, the finger states are relatively stable. This may suggest a static gesture. In this period, Thumb and Index Finger are almost straight, while Middle, Ring and Pinky Finger are bent. This suggests a static gesture involving Thumb and Index Finger, possibly Gun or Pinch.

2. **Finger Proximity (Rows 6-8):**
   Array:
   ```
   Index-Middle: [ 1.    0.   -1.   -1.    1.    1.  ]
   Middle-Ring:  [ 1.    1.    1.    1.    1.    1.  ]
   Ring-Pinky:   [ 1.    1.    1.    1.    1.    1.  ]
   ```
   - Index and Middle Finger: For frame 1-2, Index and Middle Finger are pressed together. Then they move apart, keep separated for frame 3-4, come together again, and keep pressed together for frame 5-6.
   - Middle and Ring Finger: Pressed together throughout.
   - Ring and Pinky Finger: Pressed together throughout.
   [Observation] Middle, Ring and Pinky Finger are pressed together throughout. But in frame 1-2, Index and Middle Finger are pressed together. Then Index and Middle Finger's proximity change from 1 to -1, which means they move apart. Then they keep separated for frame 3-4. Then their proximity change from -1 to 1, which means they come together again, and keep pressed together for frame 5-6.
   [Possible Gestures] Finger Proximity is not very intuitive and straightforward to indicate a gesture, so we use it to validate the gestures proposed earlier.
   In frames 1-6, we predicted the gesture to be Zoom In with Two Fingers or Click with Index Finger earlier. In this period, since Middle, Ring and Pinky Finger are pressed together throughout, and they are bent throughout from the observation of Finger Flexion, it is likely that they are not moving during the whole sequence. But there is a change in Index-Middle Finger's proximity, so the movement is likely to come from Index Finger. Both Zoom In with Two Fingers and Click with Index Finger are consistent with this observation, so they are possible to be the gesture.
   In frames 3-6, we predicted the gesture to be Gun or Pinch earlier. In frame 3-6, Index and Middle Finger change from separated to pressed together. But in Gun, Index Finger is straight while Middle Finger is bent, and they cannot be pressed together. So it is impossible to be Gun. For Pinch, Index Finger should be straight while Middle Finger is bent. But in frame 5-6, Index Finger is pressed together with Middle Finger, so it is impossible to be Pinch.

3. **Thumb Tip Contact (Rows 9-12):**
   Array:
   ```
   Thumb-Index:  [ 1.    1.   -1.   -1.    1.    1.  ]
   Thumb-Middle: [ 1.    1.   -1.   -1.   -1.   -1.  ]
   Thumb-Ring:   [ 1.   -1.   -1.   -1.   -1.   -1.  ]
   Thumb-Pinky:  [ 1.   -1.   -1.   -1.   -1.   -1.  ]
   ```
   - Thumb to Index: Initially in contact for frame 1-2, then separate, and not in contact for frame 3-4, then come together, and in contact again for frame 5-6.
   - Thumb to Middle: Initially in contact for frame 1-2, then separate, and not in contact for frame 3-6.
   - Thumb to Ring: In contact initially in frame 1, then separate, and not in contact for frame 2-6.
   - Thumb to Pinky: In contact initially in frame 1, then separate, and not in contact for frame 2-6.
   [Observation] In frame 1, Thumb's fingertip is in contact with all other fingers' fingertips. Then Thumb's fingertip contact with the other four fingers change from 1 to -1, which means Thumb moves away from all other fingers' fingertips. Then in frame 3-4, Thumb's fingertip is not in contact with any other fingers' fingertips. Then Thumb's fingertip contact with Index Finger change from -1 to 1, which means Thumb moves towards Index Finger. Then in frame 5-6, Thumb's fingertip is in contact with Index Finger, but not in contact with other fingers' fingertips.
   [Possible Gestures] Thumb Tip Contact is not very intuitive and straightforward to indicate a gesture, so we use it to validate the gestures proposed earlier.
   In frames 1-6, we predicted the gesture to be Zoom In with Two Fingers or Click with Index Finger (other gestures have been ruled out earlier). In both gestures, Thumb's fingertip should not be in contact with Index Finger's fingertip, but here they are in contact for frame 5-6. But if we only consider frame 1-4, Thumb's fingertip is not in contact with Index Finger's fingertip in the second half, so it is possible to be Zoom In with Two Fingers or Click with Index Finger.

4. **Thumb Direction (Row 13):**
   Array:
   ```
   Thumb: [ 1.    0.    0.    0.    0.    0.  ]
   ```
   - Thumb Direction: Initially pointing upward, but soon no specific direction.
   [Observation] Thumb is mostly pointing to no specific direction.
   [Possible Gestures] Based on the observation, no gesture can be guessed from this aspect because the thumb direction is unknown most of the time. But it does not rule out any gesture mentioned above, either.

5. **Palm Orientation (Rows 14-19):**
   Array:
   ```
   Left:    [ 0.    0.    0.    0.    0.    1.  ]
   Right:   [ 0.    0.    0.    0.    0.    0.  ]
   Down:    [ 0.    0.    0.    0.    0.    0.  ]
   Up:      [ 0.    0.    0.    0.    0.    0.  ]
   Inward:  [ 0.    0.    0.    0.    0.    0.  ]
   Outward: [ 1.    0.    0.    0.    0.    0.  ]
   ```
   - Mostly unknown, but at the start, it's facing outward and at the end, it's facing left.
   [Observation] Palm is facing outward in frame 1, then no specific orientation, then facing left in the last frame.
   [Possible Gestures] Based on the observation, no gesture can be guessed from this aspect because the palm orientation is almost unknown.

### Step 2: Predict One Most Possible Gesture
From the inference above, the gestures that have not been ruled out are Zoom In with Two Fingers (frames 1-4) and Click with Index Finger(frames 1-4). But there is an obvious outward movement of Thumb and Index Finger, so it is more likely to be a dynamic gesture. So the most possible gesture is:
1. [Supporting Observations]
- In frames 1-4, Middle, Ring and Pinky Finger are bent and pressed together throughout.
- Thumb and Index Finger are first bent with their fingertips in contact with each other. Then the two fingers straighten, and their fingertips are separated. This suggests a dynamic gesture where Thumb and Index Finger move away from each other.
[Supplemental Explanation] This is the same as 'Zoom In with Two Fingers' gesture in a mobile phone, usually used to zoom in the screen.
[Gesture Name] Zoom In with Two Fingers
[Time Span] 1-4

(Example 2 ends.)

Your response must follow the required format and inference steps above. REMEMBER TO OUTPUT THE PART OF THE ARRAY YOU USED.

Your Input:
{input_matrix}