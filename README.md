# Examining 3D Self-Supervised Learning for Medical Imaging

## 1. Introduction

This repository contains my CS 4782 final project, which attempts to re-implement and adapt parts of “3D Self-Supervised Methods for Medical Imaging” by Taleb et al. The paper’s main contribution is showing that 3D self-supervised pretraining can help models learn useful representations from unlabeled medical volumes before supervised fine-tuning.

My project applies this idea to a smaller-scale hippocampus segmentation task and adds a hybrid self-supervised pretraining setup that combines Rotation prediction and Contrastive Predictive Coding (CPC).

## 2. Chosen Result

The result I aimed to reproduce was the paper’s claim that 3D self-supervised pretraining improves downstream segmentation performance and data efficiency compared with training from scratch.

In the original paper, this result is shown through comparisons between models pretrained with different 3D proxy tasks and models trained from scratch. For my re-implementation, I focused on whether pretrained encoder weights could improve hippocampus segmentation performance measured using Dice score.

## 3. GitHub Contents

This repository is organized according to the required project structure. The main code is stored in `code/`, while the dataset instructions, results, poster, and final report are separated into their own folders.

```text
.
├── README.md               # Project overview and reproduction instructions
├── code/                   # Re-implementation code, scripts, notebooks, and configs
├── data/                   # Dataset files or README with dataset download instructions
├── results/                # Generated figures, tables, logs, and evaluation outputs
├── poster/                 # PDF of the in-class project poster
├── report/                 # PDF of the final 2-page project report
├── LICENSE                 # License for the repository
└── .gitignore              # Files and directories ignored by Git
```

## 4. Re-implementation Details

My re-implementation follows a simplified version of the original paper’s pipeline. I first pretrained a 3D encoder using self-supervised learning, then transferred the encoder weights into a 3D U-Net-style segmentation model for hippocampus segmentation.

My main adaptation was a hybrid Rotation + CPC model. Rotation prediction encourages the encoder to learn global 3D orientation and anatomical structure, while CPC encourages it to learn local and contextual relationships between different regions of the volume.

I implemented this using a shared 3D encoder with two self-supervised branches: one branch for rotation classification and one branch for a CPC-style contrastive objective. The model was then fine-tuned on labeled hippocampus MRI data and evaluated using Dice score, where 1 means perfect overlap between the predicted mask and ground-truth mask and 0 means no overlap.

Due to limited compute and storage, I used smaller subsets, fewer runs, and a reduced experimental setup compared with the original paper.

## 5. Reproduction Steps

To reproduce this project, first clone the repository and install the required dependencies.

```bash
git clone [<your-repo-url>](https://github.com/Smubge/cs4782_final_project.git)
cd cs4782_final_project
```

Please then run in Google Colab, upload the project folder or mount Google Drive, then update the dataset paths inside the notebook or scripts.

The general workflow is:

```text
1. Download or prepare the 3D medical imaging dataset using the instructions in data/.
2. Run preprocessing on the MRI volumes and segmentation masks.
3. Run self-supervised pretraining using the hybrid Rotation + CPC objective.
4. Save the pretrained encoder weights.
5. Transfer the encoder weights into the 3D U-Net segmentation model.
6. Fine-tune the model on labeled hippocampus segmentation data.
7. Evaluate the model using validation Dice score.
8. Save generated plots, logs, and outputs to results/.
9. Compare the pretrained model against a model trained from scratch.
```

Recommended computational resources:

```text
GPU: Colab GPU or another CUDA-capable GPU recommended
RAM: At least 12 GB preferred
Storage: Enough space for 3D MRI volumes, checkpoints, and output files
Python: 3.9+
Main libraries: PyTorch, NumPy, Matplotlib, and medical imaging utilities such as NiBabel
```

## 6. Results / Insights

The pipeline successfully trained and produced reasonable segmentation results, but I did not clearly reproduce the original paper’s strongest claim that self-supervised pretraining consistently outperforms training from scratch in low-label settings.

The clearest trend was that Dice score improved as more labeled data was used, which is expected for supervised segmentation. The hybrid Rotation + CPC model showed that the self-supervised pretraining workflow was functional, but its improvement over the scratch baseline was limited in my smaller-scale experiments.

The main reasons for this were likely limited data, limited compute, shorter pretraining time, and the difficulty of balancing the Rotation and CPC objectives. Generated plots, logs, and result summaries are included in the `results/` folder.

## 7. Conclusion

This project showed that 3D self-supervised medical imaging pipelines can be implemented at a smaller scale, but reproducing the full performance gains from the original paper is difficult.

My hybrid Rotation + CPC model was useful because it tested an adaptation of the original method instead of only copying one proxy task. Overall, the project helped me better understand 3D medical image segmentation, self-supervised learning, contrastive learning, and the practical challenges of reproducing deep learning research with limited resources.

## 8. References

Taleb, A., Loetzsch, W., Danz, N., Severin, J., Gaertner, T., Bergner, B., & Lippert, C. **3D Self-Supervised Methods for Medical Imaging**. *Advances in Neural Information Processing Systems*, 2020.

Ronneberger, O., Fischer, P., & Brox, T. **U-Net: Convolutional Networks for Biomedical Image Segmentation**. MICCAI, 2015.

PyTorch. **PyTorch: An Imperative Style, High-Performance Deep Learning Library**.

Medical Segmentation Decathlon. **Medical Image Segmentation Benchmark Datasets**.

Google Colab. **Cloud-based notebook environment used for training and experimentation**.

## 9. Acknowledgements

This project was completed as part of CS 4782 at Cornell University. I would like to acknowledge the course staff for providing guidance throughout the project and the authors of the original paper for making their work available as a reference for this re-implementation.
