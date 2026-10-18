- The builtin fixtures that come prepackaged with pytest can help do some useful things in tests easily and consistently

## Using tmp_path and tmp_path_factory 

- 왜 사용? 
	- 실제 파일 시스템에 영향을 주지 않으면서 파일이나 데이터베이스를 사용하는 코드를 테스트하기 위함
	- **즉 파일 시스템을 사용하는 코드를 실제 환경과 분리하여 안전하고 반복 가능하게 테스트하도록 도와주는 fixture** 

### to know 
- with `tmp_path_factory`, have to call `mktemp()` to get a directory. 
- `tmp_path_factory` is session scope 
- `tmp_path` is function scope

```python 
@pytest.fixture(scope="session")
def db(tmp_path_factory): 
db_path = tmp_path_factory.mktemp("cards_db)
db_ = cards.CardsDB(db_path)
yield db_ 
db_close() 
```

## Using capsys

- application code는 표준 출력(stdout), 표준 에러(error)등에 무언가를 출력하도록 설계 
	- stdout(standard Output): 프로그램의 일반적인 출력이 전달되는 스트림 
	- stderr(Standard Error): 오류 메세지나 진단 정보 등을 출력하는 별도의 스트림 

- **example** : 사용 앱의 CLI와 Python API 제공 버전이 일치하는지 체크 
```python 
def test_version(): 
process = subprocess.run(
["cards", "version"],
capture_output=True,
text=True
)
output = process.stdout.rstrip()
assert output == cards.__version__ 
```
- subprocess.run() 자체가 외부 프로세스의 출력을 캡쳐하는 기능을 제공합니다. 

- **with capsys** 
```python 
def test_greet(capsys): 
greet()
captured = capsys.readouterr()
assert captured.out == "Hello" 
```

- capsys: 표준 출력과 표준 에러에 출력된 내용을 검증할 때 유용 

- related builtin fixtures: 
	- capfd: captures file descriptors 1 and 2
	- capsysbinary: where capsys captures text, capsysbinary captures bytes 
	- capfdbinary: captures bytes on file descriptor 1 and 2 
	- caplog: captures output written with the logging package 

## Using monkeypatch 
- monkeypatch can be used to change the application code or the environment. 
- def: 프로그램 실행 중(런타임)에 객체의 속성이나 동작을 동적으로 변경하는 기법이다. 
- 사용 목적: 
	- 테스트 대상 코드가 실제 실행 환경에 의존 시, 테스트의 독립성과 재현성 확보하기 어렵다. 
	- 따라서, monkeypatch를 사용하여 테스트 대상이 의존하는 객체나 실행 환경을 임시로 교체함으로써 테스트 환경을 통제 
- 동작 원리(how) 
	- `monkeypatch.setattr()`은 객체의 속성이 참조하는 값을 임시로 변경한다. 
- 설계관점(Design for Testability) 
	- monkeypatch는 테스트 환경을 통제하는 유용한 도구지만, 내부 구현에 강하게 의존하는 패치는 테스트 유지보수성을 떨어뜨릴 수 있다. 
	- 따라서 **내부 구현을 직접 패치하기보단 설정 주입 등 테스트 가능한 인터페이스를 활용하는 것이 유리할 수 있다.** 
	- 