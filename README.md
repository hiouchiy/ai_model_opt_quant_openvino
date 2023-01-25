# OpenVINOを用いたモデルの最適化と量子化
OpenVINOを使ってセグメンテーションモデルの最適化と量子化を行い、CPU上で推論性能がどれほど向上するか体験するリポジトリ―です。
 
## Getting Started / スタートガイド
### Prerequisites / 必要条件
- Intel CPU（Core or Xeon）を搭載したマシン
    - Core: 第10世代以上
    - Xeon: 第2世代Xeonスケーラブル・プロセッサー以上
- OS: Ubuntu 18.04以降
- Docker（※以下にインストール手順記載）
### Installing / インストール
#### ホストOSのポート開放（リモートアクセスする場合のみ）
このハンズオンではJupyter Labを使用します。特にサーバーにリモートアクセスしながら実施する場合は各環境ごとの手順に則り、ホストOSのポート「8080」番を開放ください。
#### Dockerインストール
```Bash
sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
apt-cache policy docker-ce
sudo apt install -y docker-ce
sudo usermod -aG docker ${USER}
su - ${USER}
id -nG
```

#### Dockerイメージのダウンロード
```Bash
docker pull continuumio/anaconda3
```
#### Dockerコンテナの起動
コンテナはRootで起動します。また、8080番ポートをホストOSとコンテナとでバインドしておきます。
```Bash
sudo docker run -it -u 0 --privileged -p 8080:8080 continuumio/anaconda3 /bin/bash
```
以降はコンテナ上での作業になります。
#### 追加モジュールのインストール
```Bash
apt-get update
apt-get install -y wget unzip git sudo vim numactl
apt-get install -y libgl1-mesa-dev
conda create -n ov python=3.8 -y
conda activate ov
pip install --upgrade pip
pip install torch torchvision torchaudio pandas scikit-learn statistics pillow opencv-python albumentations tqdm matplotlib typing-extensions==4.4.0 jupyterlab segmentation-models-pytorch torchsummary
```
#### 本レポジトリをClone
```Bash
cd ~
git clone https://github.com/hiouchiy/ai_model_opt_quant_openvino.git
```
#### Jupyter Labの起動
```Bash
jupyter lab --allow-root --ip=0.0.0.0 --no-browser --port=8080
```
#### WebブラウザからJupyter Labにアクセス
前のコマンド実行すると以下のようなログが出力されまして、最後にローカルホスト（127.0.0.1）のトークン付きURLが表示されるはずです。こちらをWebブラウザにペーストしてアクセスください。リモートアクセスされている場合はIPアドレスをサーバーのホストOSのIPアドレスに変更してください。
```
root@f79f54d47c1b:~# jupyter lab --allow-root --ip=0.0.0.0 --no-browser
[I 09:13:08.932 LabApp] JupyterLab extension loaded from /usr/local/lib/python3.6/dist-packages/jupyterlab
[I 09:13:08.933 LabApp] JupyterLab application directory is /usr/local/share/jupyter/lab
[I 09:13:08.935 LabApp] Serving notebooks from local directory: /root
[I 09:13:08.935 LabApp] Jupyter Notebook 6.1.4 is running at:
[I 09:13:08.935 LabApp] http://f79f54d47c1b:8888/?token=2d6863a5b833a3dcb1a57e3252e641311ea7bc8e65ad9ca3
[I 09:13:08.935 LabApp]  or http://127.0.0.1:8888/?token=2d6863a5b833a3dcb1a57e3252e641311ea7bc8e65ad9ca3
[I 09:13:08.935 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 09:13:08.941 LabApp] 
    
    To access the notebook, open this file in a browser:
        file:///root/.local/share/jupyter/runtime/nbserver-33-open.html
    Or copy and paste one of these URLs:
        http://f79f54d47c1b:8888/?token=2d6863a5b833a3dcb1a57e3252e641311ea7bc8e65ad9ca3
     or http://127.0.0.1:8888/?token=2d6863a5b833a3dcb1a57e3252e641311ea7bc8e65ad9ca3
```
↑こちらの例の場合は、最後の "http://127.0.0.1:8888/?token=2d6863a5b833a3dcb1a57e3252e641311ea7bc8e65ad9ca3" です。
#### Notebookの起動
Jupyter Lab上で「ai_model_opt_quant_openvino」フォルダーに入り、その中の「Model_Optimization_and_Quantization.ipynb」を開き、後はノートブックの内容に従って進めてください。

---
## License / ライセンス
このプロジェクトは MITライセンスです。
## Acknowledgments / 謝辞
なお、本アプリケーションは[こちら](https://github.com/G21TKA01/Drone_Segmentation)をベースにアレンジを加えたものです。作者である[G21TKA01](https://github.com/G21TKA01)には事前に了承を取ったうえで使用しております。改めまして、作者の二人には素晴らしいアプリケーションを提供いただいたことに感謝いたします。
