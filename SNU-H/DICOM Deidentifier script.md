---
tags:
  - dicom
  - python
---
********
#### TODO
- 사용자가 DICOM을 업로드 하면 
- -> DICOM 이미지 메타 데이터 읽어서 **MRN(=patient_id) + CT Date 추출 ** => 마지막 csv 추출
- -> **Subj 랜덤하게 생성** (이 부분은 교수님이랑 상의해야할 듯) 
- -> Patient ID 는 Subj로, Patient Name은 Subj_CTDate 으로 deidentify, 나머지는 지금 스크립트 그대로 deidentify 
- -> deidentification 완료 후 Subj & MRN(=medical record number = patient id??) & CTDate을 CSV 파일로 따로 저장 

### 기존 deidentifier 스크립트 분석

#### deidentification (비식별화)
- 누군가의 정체성이 공개되지 않도록 예방하기 위해 사용하는 과정
- 연구 참여자들의 사생활을 보호하기 위해 비식별화함
- 데이터에 쓰이는 목적 => **데이터 익명화** 
#### deidentifier 스크립트가 하는 작업
- **dicom 이미지 비식별화**
	- dicom 폴더안의 dicom 형식의 이미지 파일들을 익명화하는 작업
	- 단일 dicom , 다중 dicom 들을 효율적으로 처리할수있음
	- 비식별화된 dicom 이미지는 환자 ID, 환자 이름, 환자 생년월일과 같은 개인 정보를 변경 
		  => PHI(Protected Health Information) 및 개인 정보를 삭제
- **dicom 이미지 메타데이터 분석**
	- dicom 파일을 분석해서 메타 데이터 추출 (patient id, name, study descripttion 등등)

#### 필요한 파일 & 디렉토리 설정
1. **DICOM 이미지 폴더**: 원본 DICOM 이미지가 있는 폴더 
	이미지 폴더의 이름은 환자 ID 와 CT 스캔 날짜를 포함한 형식 (예, KU39009_20220921)
1. **대상 폴더**: 비식별화된 DICOM 이미지를 저장할 대상 폴더의 경로를 지정
	폴더는 스크립트에 의해 생성되며, 그 안에 비식별화된 DICOM 이미지가 저장
3. **매핑 정보 CSV 파일 (-f 또는 --file 옵션)**: 매핑 정보를 제공하는 CSV 파일이 필요
	=> form 을 통해 추가적으로 받아야하는 정보???

#### 인자 전달 (form 입력)
1. 파일 디렉토리 path
```python
#python3 deid스크립트 dicom폴더
#python3 ./dicom_deidentifier_retro.py ./KU39009_20220921

python3 script.py /path/to/dicom/folder 
```
- 이러한 인자는 스크립트 내에서 각각 `src_path`, `csv_path`, `dst_path` 변수에 저장됨


### 기존 deidentifier 스크립트 수정사항

- DICOM 이미지 메타 데이터 읽어서 MRN(=일련번호?) + CT Date 추출
- deidentify 대상 수정
- terminal 명령어가 아닌, 파일 인자 전달로 돌아가는 코드
#### modified output
- De-identified dicom folder
	- patient id => subj
	- patient name => subj_ctDate
- dcm_metadata.csv
	- **subj** => string prefix + number로 구성된 uuid..
	- **MRN** => (=patient id, dcm_metadata.csv에서 추출) 
	- **CT_date** => dcm_metadata.csv에서 추출

### 수정 과정

 1. dicom 에서 MRN과 Ct_date 추출하기
- 기존 dicom 데이터 추출 함수인 analyze_dcm_series에서 수정함
- 추출할 데이터를 MRN, Ct_date로 바꾸고 나머지 필요없는 데이터들은 삭제
```python
# Initialize the dictionary for this series_uid
series_metadata_dict[series_uid] = {
	'subj': "",
	"ct_date": "",
	"MRN": "",
}
```
- 우선 initialize dictionary 를 정의해주어, not undefined의 에러를 막았다.

- 기본적으로 dcm 에서 데이터를 추출하기 위해선 라이브러리를 통해 dcm 데이터를 읽어들여야하는데,
  사용 예시를 간단히 정리해보자면, 
```python
from pydicom import dcmread

# DICOM 파일 경로
dcm_file_path = "your_dicom_file.dcm"

# DICOM 파일 읽기
dcm = dcmread(dcm_file_path)

# MRN 추출
mrn = dcm.PatientID

# CT Date 추출
ct_date = dcm.AcquisitionDate

# 결과 출력
print("MRN:", mrn)
print("CT Date:", ct_date)

```

- 그리고 나서 원하는 데이터들을 dcm 의 데이터를 추출해 해당 dict에 매핑시켜주었다.
```python
series_metadata_dict[series_uid]["subj"] = subj

series_path_dict[series_uid] = [dcm_path]
	else:
		series_path_dict[series_uid].append(dcm_path)
	try:
		series_metadata_dict[series_uid]["ct_date"] = dcm.AcquisitionDate
	except:
		series_metadata_dict[series_uid]["ct_date"] = ""
	try:
		series_metadata_dict[series_uid]["MRN"] = dcm.PatientID
	except:
		series_metadata_dict[series_uid]["MRN"] = ""
```
- 해당 cvs값을 확인해보니 잘 매핑된것을 확인하였다.



2. deidentify 함수 수정
- 기존에 코드를 살펴보면 `patientId, patientName = subj`로 비식별화된다.
- 내가 수정해야할건 `patientId` = 새로 만들 예정인 랜덤`subj`로 , `patientName` = `subj_date` 로 비식별화하는것이다.
- subj에 대한 구체적인 확정 사항이 없기때문에, 
  => subj =>  patientId로 할당
  => subj_date => subj와 ct_date를 조합해 또다른 patientName으로 할당
```python
# Overwrite PatientID, PatientName, Patient BirthDate
dcm.PatientID = subj

# Modify PatientName to Subj_CTDate
ct_date = dcm.AcquisitionDate
dcm.PatientName = f"{subj}_{ct_date}"
```
- f 라는 python의 3.6버전부터 지원되는 문자열 포매팅 기능을 사용하여, 문자열을 조합해주었다.

#### 중간 결과
정말 감격스럽게 드디어 스크립트를 돌렸다...
![[Pasted image 20230912185540.png]]

---

#### Node 와 Python 사이에 Data 전달
- electron에서 넘겨온 folder path로 deid하는 명령어를 실행한다.

- sys module을 import한다.
	- sys module은 파이썬의 인터프리터(= 소스코드를 한줄씩 읽어 처리)를 제어하는 기본 모듈
```python
import sys
```


- `sys.argv` 를 이용하여 path 를 전달받기
	- `sys.argv`
		- Python 스크립트가 외부에서 호출될 때 커맨드 라인 인수를 읽어옴
		- `sys.argv[1]`부터는 추가된 커맨드 라인 인수를 가져올수있다
		  => `spawn()`로 파이썬 인터프리터를 실행시키면서 넘겨준 path 인자를 받아온다.
```python
if len(sys.argv) != 2:
print("not valid folder path")
sys.exit(1)

folder_path = sys.argv[1]
```

- deid를 실행시키는 명령어를 정의해주고, 이를 터미널에 실행시켜주었다.
```python 
terminal_command = f"python3 ./dicom_deidentifier.py {folder_path}"
os.system(terminal_command)
```

🥹 잘 작동하는것을 확인했다.. 스크립트 수정 완료!


