---
tags:
  - dicom
  - data
date: 2023.09.10
---
### DICOM
- 디지털 의료 영상 전송 (Digital Imaging and Communication in Medicine) 의 표준
- 의료 영상 정보를 교환하고 관리하는 방법에 대한 의료 표준
- 이미지를 DICOM으로 보정하면 의료 영상 디테일을 자세히 볼수있음

### DICOM  Specification
- 복잡하고 다양한 데이터로 이루어져있다.
	- 대표적 specification
		- **File meta information header**
			- file preamble
			- DICOM prefix
			- file meta elements
		- **Information object**
			- **Data element**
				- attibute에 속함
					- tag를 통해 element 들을 구분지음
					-  ex) ![[Pasted image 20230912164830.png]]
					- **=> 실제 [[DICOM Deidentifier script]] 를 통해 추출되는 메타 데이터**