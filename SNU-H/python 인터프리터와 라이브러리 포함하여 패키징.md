
사용자의 플랫폼을 판단하여 운영체제에 따라, 파이썬 인터프리터 경로path 를 결정해준다.
```python

function getPythonPath(platform: any) {
	let pythonDir = '';
	let pythonExecutable = '';
  
	switch (platform) {
	case 'win32':
		pythonDir = 'windows';
		pythonExecutable = 'pythonw.exe';
		break;
	case 'darwin':
		pythonDir = 'mac';
		pythonExecutable = 'python3.12';
		break;
	case 'linux':
		pythonDir = 'linux';
		pythonExecutable = 'python';
		break;
	default:
		throw new Error('Unsupported OS');
	}
  
	if (app.isPackaged) {
		return path.join(
			process.resourcePath,
			'backend/python',
			pythonDir,
			pythonExecutable
		);
	}
	return path.join(
		__dirname,
		'../../backend/python',
		pythonDir,
		pythonExecutable
		);
	}
```

그리고는 python-shell을 돌릴때 python 인터프리터를 참조하여 pythonPath를 찾도록해주고, 이를 통해 python-shell로 dicom_deidentifier.py 파일을 돌릴수있도록 설정해준다.

```ts
ipcMain.on('ipc-form', async (event) => {

await checkDicomFolderPermission(folderPath);

  

const pythonPath = getPythonPath(os.platform());

  

const options = {

	pythonPath,
	
	args: [folderPath],
	
	mode: 'text' as const,

};

  

const pythonShell = new PythonShell(DICOM_PATH, options);

  

pythonShell.on('message', (message) => {

	// Python 스크립트에서 print()를 호출할 때마다 여기가 실행됩니다.
	
	console.log('Python Output:', message);
	
	// Electron 렌더러 프로세스로 메시지 전송
	
	event.reply('ipc-form-reply', { code: 'none', message });

});

  

pythonShell.end((err, code, signal) => {

	if (err) {
	
		console.error('PythonShell Error:', err, code);
		
		event.reply('ipc-form-reply', { code, message: JSON.stringify(err) });
	
	} else {
	
		console.log('PythonShell Finished:', { code, signal });
		
		event.reply('ipc-form-reply', {
		
		code,
		
		message: 'Complete Dicom Deidentification (check your directory)',
		
		});
	
	}
	
	});

});
```
