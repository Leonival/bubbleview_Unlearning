# Unlearning

Based on the BubbleView(http://bubbleview.namwkim.org/) and Beyond Memorability(https://vcg.seas.harvard.edu/publications/beyond-memorability-visualization-recognition-and-recall) experiement, we hope to design a new user study experiment for social media.

## Experiment Design

First aim: Eye-tracking (questionnaire) to figure out the important order when people read social media post?
Second aim: What components of a social media post trigger cognitive bias?

The experiment is further developed based on the Beyond Memorability experiment; there are three sections:
1. Encoding Phase: Examine 20 social media posts related to climate change, for 10 seconds each.
2. Recognition Phase: 20 posts (from the previous phase) and additional 20 new posts were presented in a blurred status, in a randomly permuted order for 2 seconds each with a 0.5 second fixation cross between posts. Participants pressed the spacebar anytime they recognized they existed the 1st phase.
3. Trigger Phase: The posts being correctly recognized in the 2nd phase will be collected. Participants will describe the post in the blurred status with three keywords or three sentences. After describing each keyword or sentence, they should click on the blurred post to choose which element makes them leave this impression.
4. Recall Phase: Participants were given 20 minutes in total to write as many descriptions of posts (bubble view) as possible. There was no limit to how much time or text length was spent on each post, nor any requirement to complete a certain number of visualizations.

## Usage Description  
We briefly describe how to use the bubbleview code based on the demo. 

Add the necessary javascript files to your html file. 
```html
  <script src="js/stackblur.min.js"></script>
  <script src="js/nouislider.min.js"></script>
  <script src="js/diff.min.js"></script>
  <script src="js/bubbleview.js"></script>
```
[`nouislider.min.js`](https://refreshless.com/nouislider/) is used for the time slider and [`diff.min.js`](https://github.com/kpdecker/jsdiff) is used for tracking description changes. You don't need `diff.min.js` if your task does not invole image description. 

The `bubbleview.js` depends on [`stackblur.js`](http://www.quasimondo.com/StackBlurForCanvas/StackBlurDemo.html) a real-time image bluring library. We slightly modified its code as the original code forcibly changs the size of canvas to match the size of an input image. In our experiments, we did not use the stackblur library, instead pre-processed images to generate blurred images using a Gaussian blur filter using Matlab's `imfilter(image, H, 'replicate')` where `H` is `fspecial('gaussian', radius, radius)`.

The `bubbleview.js` exports two functions `setup(...)` and `monitor(...)` whose definitions are:
```javascript
  function setup(imgUrl, canvasID, bubbleR, blurR, task){
    ...
  }
  function monitor(imgUrl, canvasID, bubbleR, blurR, seeBubbles, seeOriginal, clicks, maxTime){
    ...
  }
```
`bubbleR` (bubble radius) and `blurR` (blur radius) are defined in pixels. `task` is an optional user-defined callback and it will be called whenever a user clicks on canvas to generate a bubble; each bubble (center, radius, timestamp etc) will be passed as a parameter to the callback.

`clicks` is an optional list of bubbles generated based on user clicks. `monitor()` expects its format to be the same as the one passed to the user callback; that is, an item in the list should have `cx`, `cy` and `timestamp`. `maxTime` is an optional timestamp limiting the number of bubbles drawn on the canvas (see the time slider in the demo). `monitor()` returns the total number of bubbles drawn given the `maxTime` option.

