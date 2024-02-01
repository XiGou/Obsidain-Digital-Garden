 tensorboard --logdir=logs
[A ten-minute introduction to sequence-to-sequence learning in Keras](https://blog.keras.io/a-ten-minute-introduction-to-sequence-to-sequence-learning-in-keras.html)

[GitHub - mandrakedrink/ECG-Synthesis-and-Classification: 1D GAN for ECG Synthesis and 3 models: CNN, LSTM, and Attention mechanism for ECG Classification.](https://github.com/mandrakedrink/ECG-Synthesis-and-Classification) 结果很好，99+
[Inter-patient ECG heartbeat classification for arrhythmia classification: a new approach of multi-layer perceptron with weight capsule and sequence-to-sequence combination - PMC](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10569428/) 想法几乎一致，可借鉴

[ECG Classification | CNN LSTM Attention Mechanism | Kaggle](https://www.kaggle.com/code/polomarco/ecg-classification-cnn-lstm-attention-mechanism#Models) kaggle，包含三个典型模型

[Site Unreachable](https://github.com/JackAndCole/Deep-Neural-Network-For-Heartbeat-Classification)  中科大，有 focal loss 的实现


CE Loss:

Average Accuracy is: 0.7340 on test set
        N rhythm -。 Sensitivity: 0.5563, Specificity : 0.9921, Precision (PPV) : 0.9984, Accuracy : 0.6011
        S rhythm -> Sensitivity: 0.9760, Specificity : 0.5873, Precision (PPV) : 0.0840, Accuracy : 0.6018
        V rhythm -> Sensitivity: 1.0000, Specificity : 0.9990, Precision (PPV) : 0.9865, Accuracy : 0.9991
         Average -> Sensitivity: 0.8441, Specificity : 0.8595, Precision (PPV) : 0.6896, Accuracy : 0.7340

Focal loss :
2024-01-07 03:36:10.645178
Average Accuracy is: 0.9712 on test set
        N rhythm -> Sensitivity: 0.9896, Specificity : 0.6710, Precision (PPV) : 0.9634, Accuracy : 0.9569
        S rhythm -> Sensitivity: 0.0926, Specificity : 0.9905, Precision (PPV) : 0.2742, Accuracy : 0.9570
        V rhythm -> Sensitivity: 1.0000, Specificity : 0.9997, Precision (PPV) : 0.9957, Accuracy : 0.9997
         Average -> Sensitivity: 0.6941, Specificity : 0.8871, Precision (PPV) : 0.7444, Accuracy : 0.9712

Focal loss :No SMOTE
Average Accuracy is: 0.9704 on test set
        N rhythm -> Sensitivity: 0.9903, Specificity : 0.6552, Precision (PPV) : 0.9617, Accuracy : 0.9559
        S rhythm -> Sensitivity: 0.0436, Specificity : 0.9920, Precision (PPV) : 0.1739, Accuracy : 0.9566
        V rhythm -> Sensitivity: 1.0000, Specificity : 0.9986, Precision (PPV) : 0.9805, Accuracy : 0.9987
         Average -> Sensitivity: 0.6779, Specificity : 0.8819, Precision (PPV) : 0.7054, Accuracy : 0.9704
2024-01-07 03:39:31.386941