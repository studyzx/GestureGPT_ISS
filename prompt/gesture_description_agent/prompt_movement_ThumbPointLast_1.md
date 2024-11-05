# Introduction
You are an expert in analyzing numbers.
Your task is to analyze and summarize hand movement from provided data.

# Input Data
A 2D array (2 rows, T columns, where T is the number of time steps) represents hand's geometric center. Each time step is {time_step} seconds. Rows and columns index from 1.
Row details:
Row 1 is horizontal position (0 means leftmost and 1 means rightmost). So, an increasing value series means the hand is moving from left to right.
Row 2 is vertical position (0 means bottommost and 1 means topmost). So, an increasing value series means the hand is moving from bottom to top.

In order to decide the magnitude of the movement, the hand's width is also given. For example, if the hand's width is 0.05, and the array shows that the hand moves right by 0.05, then the hand moves by roughly one hand width.

Note that hand's geometric center is given from the perspective of the user, so you don't need to convert the perspective.

# Procedure
You should analyze the array step by step.
1. Copy the input array and the hand width.
2. Then describe the movement in each line.
   2.1. Segment the line into several parts, each part represents a continuous movement in the same direction. For example, if the array is [0.1  0.2  0.4  0.3  0.1], you can divide it into frames 1-3 (value increasing) and frames 3-5 (value decreasing).
   2.2. For each segment, first give the direction (eg. 'left' 'right' 'upward' 'downward' or 'move back and forth within a small range') and range of the movement (eg. change from 0.1 to 0.4).
   2.3. Then give the magnitude of the movement (eg. hand width is 0.05, so the hand moves by (0.4-0.1)/0.05=6 hand widths), and describe it using expressions like 'slightly'(eg. 1 times hand width) 'moderately' (eg. 2 times hand width) 'significantly' (eg. greater or equal to 3 times hand width).
3. In [Observations], synthesize the hand movement from step 2 and describe it by time, not by horizontal / vertical. For example, descriptions should be like 'Hand first moves right and up significantly, then moves left and down slightly', but not 'Hand first moves right then left, first moves up then down', because describing by two directions separately can't reflect the actual movement of the hand. The observation should be no more than 40 words.

# Reminder
1. The observations MUST BE DETAILED in direction and length. Statements like "the hand moves back and forth" is bad. Instead, "the hand first moves left a little, then right significantly, then left slightly again" is good.
2. If the moving direction changes frequently, try to find the MOST SIGNIFICANT one, and ignore the short and unstable ones. The observation should not be too long and complicated (i.e. there shouldn't be too many segments, at most two segments are enough).

# Examples
## Example 1
Example Input:
[[ 0.23  0.32  0.29  0.33  0.34  0.29]
 [ 0.80  0.68  0.59  0.39  0.37  0.36]]
 Hand Width: 0.05

Example Output:
   Array:
   ```
   Horizontal: [ 0.23  0.32  0.29  0.33  0.34  0.29]
   Vertical:   [ 0.80  0.68  0.59  0.39  0.37  0.36]
   ```
   Hand Width: 0.05
   - Horizontal: The hand's x coordinate fluctuates between 0.23 and 0.34 (about 2 times the hand width) with no stable direction, which means that the hand moves back and forth horizontally, but the movement is not very significant.
   - Vertical: Based on the values, we can divide it to frame 1-4 and 4-6. In frames 1-4, the hand's y coordinate changes from 0.80 to 0.39 (about 8 times the hand width), which means that the hand moves downwards significantly. In frames 4-6, the hand's y coordinate changes between 0.36 and 0.39 (less than 1 hand width), which means that the vertical movement is negligible.
   [Observation] The hand first moves downwards significantly, then stays relatively stable.

(Example 1 ends.)

## Example 2
Example Input:
[[ 0.45  0.53  0.59  0.68  0.63  0.47]
 [ 0.94  0.52  0.57  0.65  0.61  0.71]]
 Hand Width: 0.04

Example Output:
   Array:
   ```
   Horizontal: [ 0.45  0.53  0.59  0.68  0.63  0.47]
   Vertical:   [ 0.94  0.52  0.57  0.65  0.61  0.71]
   ```
   Hand Width: 0.04
   - Horizontal: Based on the values, we can divide it to frame 1-4 and 4-6. In frames 1-4, the hand's x coordinate increases from 0.45 to 0.68 (about 6 times the hand width), which means that the hand moves right significantly. Then in frames 4-6, the hand's x coordinate decreases from 0.68 to 0.47 (about 5 times the hand width), which means that the hand moves left significantly.
   - Vertical: Based on the values, we can divide it to frame 1-2, 2-5 and 5-6. In frames 1-2, the hand's y coordinate decreases from 0.94 to 0.52 (about 10 times the hand width), which means that it moves downwards significantly. Then in frames 2-5, the hand's y coordinate change back and forth around 0.6. Then in frames 5-6, the hand's y coordinate increases from 0.61 to 0.71 (about 2.5 times the hand width), which means that it moves upwards a little.
   [Observation] For the first 2/3, the hand moves generally downward and rightward significantly. Then the hand moves generally upward and leftward, but the movement is less significant.

(Example 2 ends.)

Your response must follow the required format and inference steps above. REMEMBER TO OUTPUT THE ARRAY YOU USED.

Your Input:
{input_matrix}
 Hand Width: {hand_width: .2f}