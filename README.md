# Weak to Strong Generalization for Object Detection
## Introduction
The aim of this project is to evaluate the applicability of OpenAI's weak-to-strong learning method to object detection tasks. The method is based on training a small (weak) model on a subset of the dataset with the original labels, and then using the weak model to generate labels for the full dataset. A larger (strong) model is then trained on the full dataset with the generated labels. The results are compared to a strong model trained on the full dataset with the original labels.

## Method
I trained different sizes of the YoloV5 network using the weak-to-strong learning method. The training was done in the following steps:
1. Choose two YoloV5 models, one small and one large (e.g. YoloV5s and YoloV5l)
2. Choose a dataset and make two copies of it, one with the full dataset and one with a subset (50%) of the data.
3. Train the large YoloV5 model on the full dataset with the original labels. This is the strong ceiling model and will be used as a baseline for comparison.
4. Train the small YoloV5 model on the subset of the dataset with the original labels. This is the weak model.
5. Use the weak model to generate labels (weak labels) for the full dataset.
6. Train the large YoloV5 model on the full dataset again, this time using the weak labels. This is the strong student model.
7. Compare the performance of the three models using different metrics provided by the YoloV5 library.

See `yolov5-weak-to-strong.ipynb` for the details of the implementation.

## Results
We can see that the strong student model performs better than the weak model, but worse than the strong ceiling model. This is expected, as the weak model is used to generate labels for the full dataset, and the strong student model is trained on the full dataset with the generated labels. The only exception is when we use the same YoloV5 model size for both the weak and strong models.
Some of the results are summarized in the following tables. The full results are available in the `results` directory.

### YoloV5n - YoloV5m

|          | Precision | Recall | mAP_0.5 | mAP_0.5:0.95 |
|----------|-----------|--------|---------|--------------|
| Weak     | 0.9059    | 0.8231 | 0.8951  | 0.4595       |
| Student  | 0.9219    | 0.8585 | 0.9178  | 0.5          |
| Ceiling  | 0.9375    | 0.8772 | 0.9329  | 0.524        |

### YoloV5n - YoloV5l

|          | Precision | Recall | mAP_0.5 | mAP_0.5:0.95 |
|----------|-----------|--------|---------|--------------|
| Weak     | 0.9059    | 0.8231 | 0.8951  | 0.4595       |
| Student  | 0.9109    | 0.8762 | 0.9207  | 0.5039       |
| Ceiling  | 0.9424    | 0.8998 | 0.9405  | 0.5346       |

### YoloV5m - YoloV5m

|          | Precision | Recall | mAP_0.5 | mAP_0.5:0.95 |
|----------|-----------|--------|---------|--------------|
| Weak     | 0.9186    | 0.8614 | 0.9145  | 0.5053       |
| Student  | 0.9402    | 0.866  | 0.9322  | 0.5166       |
| Ceiling  | 0.9375    | 0.8772 | 0.9329  | 0.524        |


## References
- [WEAK-TO-STRONG GENERALIZATION: ELICITING STRONG CAPABILITIES WITH WEAK SUPERVISION](https://cdn.openai.com/papers/weak-to-strong-generalization.pdf)