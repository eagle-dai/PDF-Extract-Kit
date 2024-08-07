# Install in Windows using GPU
- see https://github.com/opendatalab/PDF-Extract-Kit/blob/main/docs/Install_in_Windows_zh_cn.md

## Install ImageMagick

- Install ImageMagick
  - https://docs.wand-py.org/en/latest/guide/install.html#install-imagemagick-on-windows

- 需要修改的配置
  - PDF-Extract-Kit/pdf_extract.py:148
```
    num_workers=32
    if os.name == 'nt': # if on windows, num_workers should be 0
        num_workers = 0
    dataloader = DataLoader(dataset, batch_size=128, num_workers=num_workers)
```

## Install Dependencies
```cmd
cd PDF-Extract-Kit

python -m venv venv
venv\Scripts\activate.bat
python -m pip install --upgrade pip

pip install -r requirements+cpu.txt

# detectron2需要编译安装,自行编译安装可以参考 https://github.com/facebookresearch/detectron2/issues/5114
# 或直接使用我们编译好的的whl包
pip install https://github.com/opendatalab/PDF-Extract-Kit/raw/main/assets/whl/detectron2-0.6-cp310-cp310-win_amd64.whl
```

## Config Modifications

- PDF-Extract-Kit/configs/model_configs.yaml:2
```yaml
device: cpu
```

- PDF-Extract-Kit/modules/layoutlmv3/layoutlmv3_base_inference.yaml:72
```yaml
DEVICE: cpu
```

## Load Models
Follow instructions in models/README.md

# Run
```cmd
set KMP_DUPLICATE_LIB_OK=TRUE

# example pdf - multiples pages, with various layouts and contents Output will be saved in output folder
python pdf_extract.py --pdf assets/examples/example.pdf

# simple sample pdf
python pdf_extract.py --pdf assets/examples/sample.pdf
```
