# NTUST Edge AI (111-02) 期末專題報告_M11115017_許文珊

## 1. 作品名稱：自動去除圖像中特定類別系統

## 2. 摘要說明
本系統透過 AI 工具實現將圖片中特定類別塗抹，塗抹後可讓圖像有自然的修復效果。此系統支持輸入中出現「人、鳥、狗、貓」之圖像，可將偵測到之「人、鳥、狗、貓」從圖像中移除。

## 3. 系統簡介
### 3.1 創作發想
現今雖已有許多修圖程式內建 P 圖工具提供使用，但在使用上常常會遇到其在去背的步驟時常會有無法好好提取預處理之圖像細微特徵，在 P 圖時就會有不自然的情況發生。本提案希望利用 AI 工具實現更好之 P 圖結果，預計達成的功能為提取圖像中之物件並可將其做移除，並將移除物件後之圖像做圖像修復。

### 3.2 硬體架構
* OS：Ubuntu 20.04
* CPU：Intel(R) Core(TM) i7-9700 CPU @ 3.00GHz
* GPU：NVIDIA GeForce RTX 2080

### 3.3 工作原理及流程
* 此專案實作可分為兩大部分，首先將圖片輸入系統後，先進行物件偵測 (YOLOv8 object detection)，找尋到圖像中出現「人、鳥、狗、貓」之位置，並為其偵測結果畫出 mask 圖 (須將圖像中物件塗抹掉之位置)，最後即可將輸入圖及 mask 圖匯入 lama (image inpainting) 中，獲取將圖像中出現「人、鳥、狗、貓」之位置塗抹後的結果。
* 流程圖
    * 輸入 (input)：一有含「人、鳥、狗、貓」至少一類別之圖像。
    * 輸出 (output)：一將輸入圖中有「人、鳥、狗、貓」至少一類別之目標移除後之圖像。
![](https://imgur.com/7jQKo3o.png)

## 4. 實作步驟（於 Colab 上執行）
1. 使用 wget 指令將必要 zip 檔 M11115017_colab.zip（共 2.4 GB）下載至本地端
```
!wget --load-cookies /tmp/cookies.txt "https://docs.google.com/uc?export=download&confirm=$(wget --quiet --save-cookies /tmp/cookies.txt --keep-session-cookies --no-check-certificate 'https://docs.google.com/uc?export=download&id=1MhILX0Ke80-LdMJhblbjK9ZaKF-5hNez' -O- | sed -rn 's/.*confirm=([0-9A-Za-z_]+).*/\1\n/p')&id=1MhILX0Ke80-LdMJhblbjK9ZaKF-5hNez" -O M11115017_colab.zip && rm -rf /tmp/cookies.txt
```
2. 解壓縮 M11115017_colab.zip
```
!unzip /content/M11115017_colab.zip
```
3. 切換至 ultralytics 資料夾，下載其使用時需使用之工具
```
%cd /content/workspace/lama/ultralytics
!pip install -r requirements.txt 
```
4. 下載使用 lama 時需使用之工具
```
!pip install pytorch_lightning
!pip install kornia==0.5.0
!pip install hydra-core --upgrade
!pip install webdataset
```
5. 使用下方指令即可使用本系統，須改變之地方為圖片路徑 **infer_pic**，應改為使用者須套入之圖片路徑，此處圖片使用 /content/workspace/lama/sample_images 中之測試影像 sample1.png
```
%cd /content/workspace/lama
!python bin/predict.py refine=False device=cpu model.path=$(pwd)/big-lama  indir=$(pwd)/test_image  outdir=$(pwd)/output yolo_model=/content/workspace/lama/ultralytics/runs/detect/train/weights/best.pt infer_pic=/content/workspace/lama/sample_images/sample1.png
```




