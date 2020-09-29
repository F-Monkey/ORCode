# 参考 
[https://blog.csdn.net/qq_37674858/article/details/80340914](https://blog.csdn.net/qq_37674858/article/details/80340914)

# step:

## 1: 将图片转成.tif 格式
[generate_tif](generate_tif.png)

## 2: 生成.box文件(确保out_put_file 参数与 *** 一致，方便jTessBoxEditorFX识别)
cmd: tesseract [***.tif] [out_put_file] -l [exists_training_data] batch.nochop makebox   
eg: tesseract 111.tif my.font.exp0 -l chi_sim batch.nochop makebox

## 3: Box Editor 页面
[box editor](box_editor.png)

## 4: 创建自定义字体(0 0 0 0 0 表示字体加粗倾斜等属性为0)
echo font 0 0 0 0 0 >font_properties

## 5：开始训练.box文件，生成 .tr 文件, box_file不需要.box后缀
cmd: tesseract [***.tif] [box_file] nobatch box.train
eg: tesseract my.font.exp0.tif my.font.exp0 nobatch box.train

## 6: 生成字符文件集, 生成unicharset 文件
cmd: unicharset_extractor [box_file]
eg: unicharset_extractor my.font.exp0.box

## 7: 生成字典数据 生成[***.unicharset] 文件
cmd: mftraining -F [font_properties] -U [unicharset] -O [***.tr]  
     cntraining [***.tr]
eg: mftraining -F font_properties -U unicharset -O my.unicharset my.font.exp0.tr  
     cntraining my.font.exp0.tr

## 8: 重命名生成的(inttemp、pffmtable、normproto、shapetable) 文件，前缀一致，并合并数据集, 生成[***.traineddata]
cmd: combine_tessdata [prefix]
eg: combine_tessdata my.
        