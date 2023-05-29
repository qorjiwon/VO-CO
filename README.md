#🎤Voice-Conversion을 이용한 내 가수의 커버곡 듣기

## A. Diff-SVC 사용하기
### 👉[코랩 링크](https://colab.research.google.com/drive/18iQULcuyLp305OebGk_OYt2dZO0F_LOM)

### step.0 구글 드라이브에 데이터 업로드하기
**구글 드라이브**에**zip**의 형태로 업로드합니다. 코랩에 직접 업로드하면 시간이 굉장히 오래 걸립니다. 먼저 구글 드라이브에 파일을 업로드 한 후에 드라이브를 마운트 하는 편이 훨씬 빠르고, 이후에도 계속 사용할 수 있기 때문에 유용합니다.

그다음 코랩으로 돌아와 **GPU 타입을 체크**하고 드라이브를 마운트 해주세요.

### step1.
여기서 step은 코랩에 적혀있는 단계를 따릅니다. 

⚠️반드시 <u>agree에 체크</u>하고 실행하세요. 그렇지 않으면 모델이 다운로드가 되지 않습니다.
![image](https://github.com/qorjiwon/VO-CO/assets/82700743/425ef103-3a43-488d-896e-29a74c56606c)


실행시키고 난 후 다음과 같은 Exception이 발생하면서 **런타임 에러**가 발생합니다. 하지만 이는 **에러가 아니니** <u>그냥 넘어가세요.</u>
![image](https://github.com/qorjiwon/VO-CO/assets/82700743/ebff83c1-82b0-42ee-8094-52acf77959a3)

한 번에 뒤에 셀들까지 실행시켰을 경우, 해당 셀을 실행시키고 나면 뒤에 셀들이 중단되니 에러가 난 후 뒤에 셀들을 잊지 말고 실행해 주세요.

### step2.
이름을 설정하고 이전에 드라이브에 올려둔 데이터 셋의 경로를 복사합니다.
![image](https://github.com/qorjiwon/VO-CO/assets/82700743/2b6c2333-3935-4360-8851-9319ae6764d9)
만약 이 단계에서 코랩이 zip을 찾지 못한다면 zip이 제대로 압축되지 않거나 손상됐을 확률이 큽니다. 다른 프로그램을 이용하여 다시 압축해 보세요. step2-A는 패스합니다.

### step3. 
save_dir에 나의 구글 드라이브에 **모델을 저장하고 싶은 경로**를 입력하세요. 참고로 모델은 **2000step**마다 저장이 되며, 디렉토리는 따로 설정하지 않을 경우에 checkpoints에 저장이 됩니다. checkpoints는 /content/diff-svc/checkpoints에 있으며 런타임이 종료되면 날아갈 수 있으니 주의하세요.

또 만약 데이터 셋이 1시간 이하로 짧은 경우에 endless_ds를 체크하세요.
![image](https://github.com/qorjiwon/VO-CO/assets/82700743/5fc488dc-b400-4112-a329-fcb627eb0d2a)


### step 4~
이후에는 그냥 실행시키면 됩니다. 전처리 과정도 꽤 오래 걸립니다. (30분 정도) 참고하세요.


----------
## B. How to Resume Training 

step 1까지 위를 보고 진행하세요.

### step2 
step 2-A를 진행할 겁니다. 우리는 위 단계에서 이미 데이터를 전처리하는 과정을 거쳤고 이를 저장했기 때문에 굳이 시간이 오래 걸리는 전처리 과정을 한 번 더 거칠 이유가 없습니다. 
preprocessed_data_location에 가수이름_**binary_data.7z 파일의 경로**를, config_location에는 **config_nsf.yaml 파일의 경로**를 입력하세요.
![image](https://github.com/qorjiwon/VO-CO/assets/82700743/66ca356e-8242-4c2a-80ea-b93f7159d5e6)
<u>config.yaml이 아니라 con_nsf.yaml입니다.</u> 주의하세요.

**폴더의 이름에 띄어쓰기가 있으면 폴더를 인식 못 할 수도 있습니다. 꼭 확인해 주세요**

### step3.
**save_dir**에는 **모델을 저장할 경로**, **ckpt_directory**에는 가장 최근의 **ckpt 파일의 경로**를 입력합니다. 데이터 셋의 크기가 1시간 미만으로 작을 때는 endless_ds를 체크합니다.
![image](https://github.com/qorjiwon/VO-CO/assets/82700743/88a06aa5-276d-4406-8c8b-57b45c2dc583)

### step4~
step4는 실행하지 않고 넘어갑니다. 
step5와 step6을 실행합니다.
step5에서는 모델의 진척 상황을 시각화합니다. 이전 모델들도 같이 넣어두면 추이를 확인할 수 있습니다. 참고하세요.

## C. How to laod the model 
### 0. step1까지 진행해주세요

### 1. step6
model_path에는 로드할 모델의 경로를  
config_path에는 config.yaml 파일의 경로를  
wav_in에는 목소리를 덮어씌울 reference voice의 경로를 입력해주세요. (사진에서는 마크툽 목소리로 성시경의 노래를 덧씌울때 성시경의 목소리가 reference voice)

crepe는 좀 더 안정적인 음성을 출력하지만, 발음이 뭉개질 수 있습니다.  경험적으로 출력된 음성에 reference voice가 과도하게 섞이면 crepe를 체크하면 좀 더 나은 결과가 나옵니다. 
pe는 좀 더 자연스러운 목소리가 출력되지만, pitch가 맞지 않는 결과물이 출력될 수 있습니다. 
![image](https://github.com/qorjiwon/VO-CO/assets/44426921/41c4eba1-2608-4521-8ec9-23f9e8460614)

결과물은 /content/diff-svc/results에 저장됩니다. 우클릭을 하여 다운로드 받을 수 있습니다.
![image](https://github.com/qorjiwon/VO-CO/assets/44426921/ee7f52d9-5363-4b01-829f-d0d66231fcd3)

***
👉[Diff-SVC 모델 사용 가이드 원본](https://docs.google.com/document/d/1nA3PfQ-BooUpjCYErU-BHYvg2_NazAYJ0mvvmcjG40o/edit#heading=h.x5mtoparsl14)
