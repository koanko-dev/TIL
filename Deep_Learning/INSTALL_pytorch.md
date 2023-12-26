실행중인 프로세스 확인 `ps -e`
프로세스 종료 `kill <port num>`
가상환경 종료 `conda deactivate`

1. gpu 정보에 CUDA 버전 확인
`nvidia-smi`

2. yolo 전용 가상환경 생성. 가상환경명: yolov5Test
`conda create --name yolov5Test python=3.8`

3. 가상 환경 안으로 들어가기
`conda activate yolov5Test`

4. 파이토치 설치: start locally > stable, linux, conda, python, cuda11.7
`conda install pytorch torchvision torchaudio pytorch-cuda=11.7 -c pytorch -c nvidia`

5. 가상 환경(yolov5Test)을 만들면 주피터 노트북을 연결
`python -m ipykernel install --user --name <가상환경명> --display-name "커널출력명(주피터노트북에 어떻게 나타나게 하는지)"`
`python -m ipykernel install --user --name yolov5Test --display-name "yolov5Test"`

5-1. /home/ubuntu/anaconda3/envs/yolov5Test/bin/python: No module named ipykernel에러 뜨면
`pip install ipykernel`

그다음 주피터 노트북 켜주면 됨. 이제 yolov5Test 사용 가능.
`nohup jupyter-notebook --ip=0.0.0.0 --no-browser --port=8xxx &`
다른 커널 쓰고 싶으면 액티베이션 Python3으로 해놓고 들어오면 됨


## yaml 파일
nc: 디텍션해야 할 객체 개수
train: 여기에 경로와 함께 써 있는 txt파일 안에는 => 학습에 사용해야 할 이미지에 파일 명이 들어가야 함.
test: train과 같음.