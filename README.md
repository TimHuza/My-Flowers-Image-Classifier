# 🌷🌹**PROJECT PLAN: Flowers Image Classifier**

## **PHASE 1 — Prepare Your Dataset**

### 1.1. Get a cats vs dogs dataset

You can:

* Use Kaggle’s “Cats and Dogs” dataset
* Or any folder with two class folders:

  ```
  datasets/cats_dogs/
      cats/   (all cat images)
      dogs/   (all dog images)
  ```

### 1.2. Clean the dataset (optional but recommended)

Check for:

* corrupt images
* duplicates
* wrong labels (dog image inside cats folder)

### 1.3. Decide image size and batch size

Example (you choose your own):

* `img_height = 180`
* `img_width = 180`
* `batch_size = 32`

Write these down — you’ll use them in the loading step.

---

## **PHASE 2 — Load Your Dataset With Keras**

You already know how to do this from the flowers project.

Your plan:

1. Use `image_dataset_from_directory`
2. Use `validation_split` (e.g., `0.2`)
3. Create:

   * `train_ds`
   * `val_ds`
4. Print the `class_names` — check they show: `["cats", "dogs"]`

### Think about:

* Do you want the split to be saved (use `seed=123`)?
* Is your dataset very large (maybe avoid `.cache()` in RAM)?

---

## **PHASE 3 — Optimize Your Input Pipeline**

Apply the same performance steps you learned:

* `.cache()`
* `.shuffle()`
* `.prefetch(AUTOTUNE)`

### Important thought:

Is your dataset big?
→ If yes, do **not** use `.cache()` without a filename.
→ Instead use `.cache("cache_file")`.

---

## **PHASE 4 — Design Your Model**

Here is where you get creative.

You need to make these decisions:

### 4.1. Will you use data augmentation?

Examples you can choose:

* `RandomFlip`
* `RandomRotation`
* `RandomZoom`

Ask yourself:

* Do cats/dogs flip horizontally naturally? (yes)
* Should you rotate too much? (probably not — 10% is safe)
* Should you flip vertically? (no — cats upside-down look wrong)

### 4.2. Architecture planning

Create a small CNN similar to your flowers model.

Decide:

* How many Conv2D layers (2, 3, or 4?)
* How many filters (16 → 32 → 64?)
* Use Dropout or BatchNormalization?
* Dense layer size (128? 256?)

### 4.3. Output layer

Since this is **binary** classification (cats vs dogs), choose one approach:

**Option A: 1 output + sigmoid**

* Final layer: `Dense(1, activation="sigmoid")`
* Loss: `BinaryCrossentropy`
* Labels: 0 or 1

**Option B: 2 outputs + softmax**

* Final layer: `Dense(2, activation="softmax")`
* Loss: `SparseCategoricalCrossentropy`

Both work, but beginners usually choose **A**.

Pick one and stick with it.

---

## **PHASE 5 — Compile Your Model**

You must choose:

* Optimizer (Adam is good)
* Loss function (based on A or B above)
* Metric (`accuracy`)

Think:

* Do you want to experiment with learning rates?
  Example: `Adam(learning_rate=1e-4)`

---

## **PHASE 6 — Train Your Model**

Training plan:

1. Choose number of epochs (start with 10)
2. Pass `train_ds` and `val_ds`
3. Store the `history`
4. Plot accuracy and loss curves (like in flowers project)

When looking at the graphs, ask yourself:

* Is training accuracy much higher than validation accuracy? → overfitting
* Is both accuracy low? → underfitting
* Is the model improving? Or flat?

This analysis teaches real ML intuition.

---

## **PHASE 7 — Evaluate and Inspect Results**

### Things to do:

✔ Check final accuracy
✔ Use `model.predict()` on some images:

* Try your own cat or dog photos
* See if it gets confused
* Print prediction probabilities
  ✔ Inspect wrong predictions (important for learning)

### Optional advanced step:

Make a **confusion matrix**:

* Cats predicted as cats
* Cats predicted as dogs
* Dogs predicted as dogs
* Dogs predicted as cats

This reveals where the model struggles.

---

## **PHASE 8 — Save and Reuse Your Model**

Just like before:

* Save the trained model
* Load it for inference later
* Build a simple script that takes an image and outputs “cat” or “dog”
