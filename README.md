### intro
- Repo for evalute paper: [Restormer: Efficient Transformer for High-Resolution Image Restoratio](https://arxiv.org/abs/2111.09881).
- Code adapted from [author released notebook](https://colab.research.google.com/drive/1C2818h7KnjNv4R1sabe14_AYL7lWhmu6?usp=sharing).

### new data
- Created dataset: [./test_data/metadata.json](./test_data/metadata.json)
  - alpha ranges [0.1-0.5, 0.5-0.8, 0.8-1.0]
  - mask random scale [1.0-1.2]
  - mask random crop
  - size: 3x100 = 300 images (3 alpha ranges, other parameters fixed)
- Script: [./image_gen.ipynb](./image_gen.ipynb)
- Visual: <br>
  <img src="./assets/corrupted_data.png" width=70%>

### eval results
- Script: [./restormer_eval.ipynb](./restormer_eval.ipynb)
- Metrics (不确定对不对）：
    ```python


          def rgb_to_y(self, img):
              """Convert RGB to Y channel - matches MATLAB rgb2ycbcr"""
              from skimage.color import rgb2ycbcr
              img_ycbcr = rgb2ycbcr(img)  # Expects float [0,1] or uint8 [0,255]
              return img_ycbcr[:, :, 0]


          from skimage.metrics import peak_signal_noise_ratio, structural_similarity
          from skimage import img_as_ubyte
                # Convert to numpy
                restored_np = restored.permute(0, 2, 3, 1).cpu().detach().numpy()
                restored_np = img_as_ubyte(restored_np[0])

                # Calculate RGB metrics
                psnr_rgb = peak_signal_noise_ratio(original_np, restored_np, data_range=255)
                ssim_rgb = structural_similarity(original_np, restored_np, channel_axis=2, data_range=255)
                mae = np.mean(np.abs(original_np.astype(float) - restored_np.astype(float)))
                mse = np.mean((original_np.astype(float) - restored_np.astype(float)) ** 2)

                # Calculate Y channel metrics
                original_y = self.rgb_to_y(original_np)
                restored_y = self.rgb_to_y(restored_np)
                psnr_y = peak_signal_noise_ratio(original_y, restored_y, data_range=255)
                ssim_y = structural_similarity(original_y, restored_y, data_range=255)
    ```
- Table ([./assets/table.tex](./assets/table.tex)): <br>
  <img src="./assets/table.png" width=90%>
- Visual: <br>
  <img src="./assets/eval_comparison.png" width=70%>
