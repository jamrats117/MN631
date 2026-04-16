## 1) Structured summary

### 1.1 What computer vision is

* Vision = ability to understand **what is where by looking**
* Human vision is not only object recognition
* It also includes:

  * motion
  * temporal change
  * scene dynamics
  * spatial relationships
* Computer vision tries to give this ability to machines

### 1.2 Why vision is hard for computers

The same object can look different because of:

* viewpoint
* scale
* rotation
* deformation
* lighting
* occlusion
* different cameras
* orientation

So good models need some level of **robustness / invariance** to these changes.

---

### 1.3 How images are represented

* **Grayscale image** = 2D array of numbers
* **Color image** = 3D array: height × width × channels
* RGB image has 3 channels:

  * red
  * green
  * blue

This is important because CNNs operate directly on this spatial numeric structure.

---

### 1.4 Main ML task types in this lecture

#### Classification

Predict a discrete label.

Examples:

* face / not face
* taxi
* digit 0–9

#### Regression

Predict a continuous value.

Examples:

* steering angle
* control parameter

---

### 1.5 Feature detection idea

To classify something, the model must detect distinguishing features.

Examples:

* face → eyes, nose, ears
* house → doors, windows, steps
* X shape → two diagonals + crossing

Core idea:

1. identify important features
2. detect them
3. use them for prediction

Traditional ML often uses **handcrafted features**.
Deep learning learns features **from data**.

---

### 1.6 Why fully connected networks are weak for images

To use a fully connected network on an image, you must **flatten** the image into 1D.

Problems:

* destroys spatial information
* loses neighborhood relationships
* creates too many parameters
* computationally expensive
* poor design for visual structure

**Key exam point:**
Flattening early is bad because images are spatial.
Flattening later is okay after the model has already extracted spatial features.

---

### 1.7 Local connectivity

Instead of connecting every pixel to every neuron:

* each neuron sees only a **local patch**
* neighboring output neurons come from neighboring input patches
* overlapping patches preserve continuity

This preserves spatial structure and reduces parameter count.

---

### 1.8 Convolution

Convolution is the main operation in CNNs.

A **filter** slides across the image:

* compare filter with local patch
* multiply elementwise
* sum
* add bias
* apply nonlinearity

This lets the model detect local patterns like:

* edges
* corners
* diagonals
* texture changes

---

### 1.9 Filters

A filter is a small matrix of weights.

It can represent patterns such as:

* diagonal line
* edge
* crossing
* brightness transition

Important:

* historically, filters could be hand-designed
* in CNNs, filters are usually **learned automatically**

**Filter choice controls what gets detected.**

---

### 1.10 Hierarchical learning in CNNs

CNNs learn features layer by layer:

* early layers → edges, intensity changes
* middle layers → parts like eyes, nose, ears
* deeper layers → larger object structures like full faces

This is why depth matters.

---

### 1.11 Three main CNN operations

#### 1. Convolution

Extract local features.

#### 2. Nonlinearity

Usually ReLU. Adds expressivity.

#### 3. Pooling

Downsamples feature maps and enlarges effective receptive field.

---

### 1.12 ReLU

ReLU is commonly used after convolution.

Behavior:

* negative → 0
* positive → unchanged

Interpretation:

* acts like a threshold
* suppresses negative responses
* keeps positive evidence

---

### 1.13 Pooling

Pooling reduces spatial size.

#### Max pooling

Keeps only the maximum in a patch.

#### Mean pooling

Averages values in the patch.

Why pooling is useful:

* reduces dimensionality
* increases effective receptive field
* supports multiscale feature detection

Important nuance:

* max pooling is harsher on gradients
* mean pooling gives smoother gradient flow

---

### 1.14 Feature maps and volumes

One filter produces **one feature map**.

Multiple filters produce a **volume** of feature maps.

Example:

* 32 filters → depth 32 output
* next layer 64 filters → depth 64 output

---

### 1.15 Softmax

After feature extraction, the model may:

* flatten the output
* use a final classifier
* apply softmax

Softmax converts scores into class probabilities that sum to 1.

Use softmax for **classification**, not regression.

---

### 1.16 General CNN pipeline

Typical classification CNN:

1. input image
2. convolution
3. ReLU
4. pooling
5. convolution
6. ReLU
7. pooling
8. flatten
9. final classifier
10. softmax

Example mentioned:

* 32 feature maps
* then 64 feature maps
* flatten
* final 10-class prediction

---

### 1.17 Why training data matters

CNNs learn from the distribution they see.

Example:

* train only on upright faces
* test on sideways faces
* performance should be poor

If both orientations appear in training, the model can learn both.

**Exam point:**
Generalization depends strongly on training data coverage.

---

### 1.18 CNNs are not just for classification

A CNN can be split into:

1. **feature extractor**
2. **task-specific head**

Same feature extractor can support:

* classification
* object detection
* segmentation
* regression
* control models

---

### 1.19 Classification vs object detection vs segmentation

#### Classification

Predict one label for the whole image.

Example:

* image → taxi

#### Object detection

Predict:

* class
* location
* bounding box
  for each object

Example:

* image → taxi + box

#### Semantic segmentation

Predict class for **every pixel**.

Example:

* cow image → cow / grass / tree / sky per pixel

---

### 1.20 Object detection intuition

Naive method:

* generate many boxes
* classify each box independently

Problem:

* too slow
* too many possible boxes
* poor scalability

Better approach:

* propose likely regions first
* then classify them

R-CNN idea:

* use a region proposal mechanism
* share features between proposal and classification
* more efficient and more aligned end-to-end

---

### 1.21 Segmentation intuition

Segmentation is still classification, but:

* output stays in **2D**
* prediction is made for every pixel
* network often uses:

  * downsampling in encoder
  * upsampling in decoder

So unlike standard classification, segmentation does **not** collapse the whole image to one final label.

---

### 1.22 CNNs for autonomous driving

Inputs:

* camera image
* map-like image

Pipeline:

* extract features from each input
* combine features
* regress control outputs

Output:

* continuous control behavior, such as steering-related prediction

This shows CNNs can be used for **regression/control**, not only classification.

---

### 1.23 Big-picture takeaway

Across all tasks, the foundation is the same:

* images as arrays
* local feature extraction
* learned filters
* hierarchical representation
* convolution + nonlinearity + pooling
* optional upsampling for segmentation
* task-specific output head

---

## 2) Key formulas / operations

## 2.1 Image representation

### Grayscale

`Image ∈ R^(H × W)`

Use when:

* image has one intensity channel

### Color

`Image ∈ R^(H × W × C)` where usually `C = 3`

Use when:

* RGB information matters

---

## 2.2 Convolution operation

For one filter at one location:

1. take local patch
2. do elementwise multiplication
3. sum all values
4. add bias
5. apply activation

Compact form:

`output = activation(sum(patch ⊙ filter) + b)`

Where:

* `⊙` = elementwise multiplication
* `b` = bias

### When to use

Use convolution when:

* input has spatial structure
* local pattern detection matters
* you want parameter sharing and locality

---

## 2.3 ReLU

`ReLU(x) = max(0, x)`

### When to use

Use after convolution or linear layers to:

* introduce nonlinearity
* suppress negative activations
* improve expressive power

---

## 2.4 Max pooling

For a patch, output:

`max(values in patch)`

Example:

* `2 × 2 → 1 × 1`

### When to use

Use when you want:

* downsampling
* strongest local activation
* some translation tolerance

---

## 2.5 Mean pooling

For a patch, output:

`average(values in patch)`

### When to use

Use when you want:

* downsampling
* smoother gradients than max pooling

---

## 2.6 Softmax

For class score `z_i`:

`softmax(z_i) = e^(z_i) / Σ_j e^(z_j)`

Properties:

* outputs are positive
* outputs sum to 1

### When to use

Use for:

* multiclass classification
* final probability distribution over classes

Do **not** use softmax for continuous regression targets.

---

## 2.7 Classification vs regression target

### Classification

Output = one of `K` classes

### Regression

Output = continuous value

Examples:

* class label → classification
* steering angle → regression

---

## 2.8 Example CNN architecture

`Input → Conv → ReLU → Pool → Conv → ReLU → Pool → Flatten → Linear/Classifier → Softmax`

Example from lecture:

* first conv layer: 32 feature maps
* second conv layer: 64 feature maps
* final output: 10 classes

### When to use

Use this as the default skeleton for basic image classification questions.

---

## 3) Quick examples

### 3.1 Face classification

Input image → classify as face / not face
Detected features may include:

* eyes
* nose
* ears

---

### 3.2 President classification

Input image of Abraham Lincoln or George Washington
Output = predicted class

---

### 3.3 X-pattern detection

Important local features:

* left-to-right diagonal
* right-to-left diagonal
* crossing

If these local patterns are present, the model can classify the image as an X even if the X is skewed or shifted.

---

### 3.4 Why flattening fails

Image → flatten → fully connected network

Problem:

* neighboring pixels become just distant entries in a vector
* spatial relationships disappear

---

### 3.5 Early vs deep CNN features

* early layer → edges
* mid layer → eyes / nose / ears
* deep layer → whole face

---

### 3.6 Max pooling

Given a 2×2 patch:

* take the largest value
* discard the rest

---

### 3.7 Classification vs detection

#### Classification

Image with taxi → output `taxi`

#### Detection

Image with taxi → output:

* taxi
* bounding box
* location

---

### 3.8 Segmentation

Cow scene → output per-pixel labels:

* cow
* grass
* tree
* sky

---

### 3.9 Autonomous driving

Inputs:

* front camera
* map image

Output:

* continuous control signal / steering-related output

---

### 3.10 Data distribution issue

Train only on upright faces
Test on sideways faces
Expected: poor performance

Reason:

* model learned only upright-face patterns

---

## 4) Exam tips

### 4.1 Always state why CNNs are better than fully connected networks for images

Best points to mention:

* preserve spatial information
* local connectivity
* fewer parameters
* shared filters
* hierarchical feature learning

---

### 4.2 If asked “why convolution works,” say both

* it detects local patterns
* it preserves spatial structure

Do not give only one of these.

---

### 4.3 If asked “why pooling is used,” mention both

* downsampling
* larger effective receptive field

Bonus point:

* supports multiscale detection

---

### 4.4 If asked “why deep learning instead of handcrafted features,” say

* handcrafted features require human design
* deep learning learns useful features directly from data
* learned features can be hierarchical and task-specific

---

### 4.5 Know the difference between tasks

Memorize this:

* **classification** = one label for image
* **detection** = labels + boxes
* **segmentation** = label per pixel
* **regression/control** = continuous output

---

### 4.6 If asked about robustness/generalization

Mention:

* depends on training data diversity
* model learns what it sees
* missing orientations/scales in training can hurt test performance

---

### 4.7 When writing about segmentation

Make sure to say:

* output remains 2D
* classification is per pixel
* often uses upsampling in the decoder

---

### 4.8 When writing about object detection

Mention why naive box classification is bad:

* too many boxes
* expensive
* doesn’t scale

Then mention region proposal / shared-feature idea.

---

### 4.9 For open-book exam answers, use this short template

If asked to explain a CNN-based task:

1. define input representation
2. explain convolution/filter
3. explain ReLU
4. explain pooling
5. explain final task head
6. explain output type
7. mention training learns filters by backpropagation

---

## 5) Common mistakes

### Mistake 1: Saying convolution and pooling are the same

They are different.

* convolution = feature extraction
* pooling = downsampling

---

### Mistake 2: Saying CNNs automatically handle all rotations/orientations

Not necessarily.

They only learn what training data supports.

---

### Mistake 3: Forgetting why flattening early is bad

Early flattening destroys spatial structure.

Flatten only after spatial features have been extracted.

---

### Mistake 4: Using softmax for regression

Wrong.

Use softmax for class probabilities, not continuous outputs.

---

### Mistake 5: Saying object detection is just classification

Wrong.

Detection also predicts location and bounding boxes, and the number of objects may vary.

---

### Mistake 6: Saying segmentation is just object detection

Wrong.

Segmentation predicts a label for every pixel, not just a box.

---

### Mistake 7: Forgetting multiple filters

One convolutional layer usually has many filters, not one.

That is why output is a stack / volume of feature maps.

---

### Mistake 8: Ignoring the role of nonlinearity

Without nonlinearity, stacking layers loses expressive power.

ReLU is crucial.

---

### Mistake 9: Assuming max pooling is always best

Lecture explicitly notes alternatives like mean pooling and mentions gradient issues with max pooling.

---

### Mistake 10: Describing CNNs only as classifiers

CNN backbones can also support:

* detection
* segmentation
* regression
* control

---

## 6) Ultra-short memory section

### Core pipeline

`Image → Conv → ReLU → Pool → Repeat → Flatten or Upsample → Task head`

### Core logic

* images have spatial structure
* CNNs preserve that structure
* filters learn local patterns
* deeper layers combine simple patterns into complex ones

### Task outputs

* classification → label
* detection → label + box
* segmentation → per-pixel labels
* regression → continuous value

### Why CNNs

* local connectivity
* fewer parameters
* shared filters
* hierarchical learning

---

## 7) Final one-paragraph exam answer

A CNN is a deep learning model designed for images, which are represented as 2D or 3D arrays of pixel values. Unlike fully connected networks, CNNs preserve spatial information by using local connectivity and convolutional filters that slide over image patches to detect local patterns such as edges, lines, and object parts. After convolution, nonlinearities such as ReLU increase model expressivity, and pooling downsamples feature maps to enlarge the effective receptive field. Stacking layers allows hierarchical feature learning, from simple edges to complex structures such as faces. The same CNN backbone can support classification, object detection, segmentation, and regression/control tasks depending on the output head. Robust performance depends strongly on the diversity of the training data.

---

## 8) Final verdict on completeness

This cheat sheet is **sufficient for exam use** for the lecture content you provided.

What it now does well:

* covers all major concepts
* keeps formulas minimal but usable
* separates task types clearly
* includes pitfalls and answer strategies
* works as a single open-book reference

The only thing not deeply covered is **formal notation details** like output-size calculations for convolution or pooling, because they did not clearly appear in your transcript chunks. If your exam may ask numerical output shapes, that would be the one area to add next.
