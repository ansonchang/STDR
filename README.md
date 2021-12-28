## 文字定位與辨識 - 高階賽 程式說明文件


## 1. 套件安裝

安裝 PaddleOCR 套件在 PaddleOCR 目錄, 請參閱 [github](https://github.com/PaddlePaddle/PaddleOCR)

​	

## 2. 資料前處理

### 2.1 文字檢測資料前處理

### get_det_file.ipynb

- 將 train data json file 轉換為 PaddleOCR detection model 可以存取的 txt file 格式, 並且切分 90% training dataset 以及 10% validation dataset. 
- 針對training dataset / validation dataset 各自產生 txt files.

### 2.2 文字辨識資料前處理

### cropImg4rec.ipynb

- 先萃取 get_det_file 跑出來的 txt file, 將 bounding box 與 label 資料轉成 csv format. 
- 針對 csv bounding box crop 為小圖
- 接著作影像前處理, 包含影像拉正, 影像轉 90 度等

### get_rec_file.ipynb

- 將 train data csv 轉換為 PaddleOCR 可以存取的 txt file 格式, 並且切分 80% training dataset 以及 20% validation dataset. 

  

## 3. 模型與參數設定

### 3.1 文字檢測模型與參數

採用 PaddleOCR 實作的 DBNet 模型, 參數請參閱 PPOCR_det.yml

### 3.1 文字辨識模型與參數

採用 PaddleOCR 實作的 CRNN 模型, 參數請參閱 rec_chinese_cht_lite_train.yml



## 4. 開始訓練

### 4.1 文字檢測模型訓練

針對  DBNet  model 展開訓練 , 在 PaddleOCR 目錄下執行以下 script:

* python3 tools/train.py -c configs/det/PPOCR_det.yml

執行完, 會產生 runs 目錄. 模型權重會產生在  ./output/PPOCRv2_det/ 目錄下

### 4.2 文字辨識模型訓練

針對  CRNN model 展開訓練 , 在 PaddleOCR 目錄下執行以下 script:

* python3 tools/train.py -c configs/rec/rec_chinese_cht_lite_train.yml

執行完, 模型權重會產生在  ./output/rec_chinese/ 目錄下



訓練模型與預測都需要字典, 字典請參閱以下檔案

-  config/cht_dict.txt



## 5. 開始預測

將文字檢測模型與文字辨識 pre-trained models 轉為 inference models

(1) 文字檢設模型為 inference model, 在 PaddleOCR 目錄下執行以下 script:

* python3 tools/export_model.py -c configs/det/config.yml \
               -o Global.pretrained_model=output/PPOCRv2_det/best_accuracy  Global.save_inference_dir=inference/det_db/

(2) 文字辨識模型為 inference model, 在 PaddleOCR 目錄下執行以下 script:

* python3 tools/export_model.py -c configs/rec/rec_chinese_cht_lite_train.yml \
             -o Global.pretrained_model=output/rec_chinese/best_accuracy  Global.save_inference_dir=inference/rec/

### e2e_predict.ipynb

- 執行此 code 預測最後結果. 將文字檢測與文字辨識 inference model 輸入到 e2e_model  的參數中, 執行預測任務.



## 6. 打包結果上傳

透過 e2e_predict.ipynb 將預測結果轉為 submit csv 的格式並上傳.
