1. Básico
   
	- notas - 
		- Rodar testes
		- 
```
pytest test_nome_do_doc.py
```

- verbose
- para rodar de forma a obter mais dados, basta colocar a opção (-v)

```
pytest -v test_nome_do_doc.py
```
- instalando via pip
```
$ pip3 install -U virtualenv
$ python3 -m virtualenv venv
$ source venv/bin/activate
$ pip install pytest
```
sobre os testes, para o pytest identificar testesprecisa começar com test_ assim qd rodar o comando$pytest ele irá buscar na pasta atual e subpastas. 	posso rodar passando o nome dos teste q desejo testar

```
S pytest task/test_nome_do_doc.py task/test_nome_do_doc2.py
```
posso passa a pasta onde estão os teste
```
$ pytest task
```
ou simplesmente passando pytest dentro da pasta onde estão os teste
```
$ cd past/destino
$ pytest
```
todas irão me dar o mesmo resultado

PHASES
- Call
	- 
- Setup - fixtures
- Teardown - fixtures
test discovery

	Ele irá buscar 3 coisas
	- arquivos nomeados test_<something.py> or <something_test.py
	- Métodos e funções nomeados test_<something>
	- Classes testes nomeados Test<Something>
Possíveis resultados

- PASSED(.) - ran successfully
- FAILED(F) - not run successfully (or XPASS + strict)
- SKIPPED (s) - Test was skipped. Using either the @pytest.mark.skip() or pytest.mark.skipif()
- xfail (x) - test was not suposed to pass, ran, and failed. @pytest.mark.xfail()
- XPASS (X) - test was not suposed to pass, ran, and passed
- ERROR (E) - exception happened

Using Options

- -v or --verbose
  
- --collect-only
  
	- pytest --collect-only
	- mostra os testes disponíveis para rodar
```
pytest --collect-only                                                                                                                  [5d6h52m]
================================================================== test session starts ===================================================================
platform linux -- Python 3.8.3, pytest-6.2.2, py-1.10.0, pluggy-0.13.1
rootdir: /home/serlus/repositorios_estudos/pytest_code/ch1
collected 6 items                                                                                                                                        

<Module test_one.py>
  <Function test_passing>
<Module test_two.py>
  <Function test_failing>
<Module tasks/test_four.py>
  <Function test_asdict>
  <Function test_replace>
<Module tasks/test_three.py>
  <Function test_defaults>
  <Function test_member_access>
```
essa opção é util com o -k para mostrar como elas funcionam.
- -k "expression"
  
	- pytest -k "expression"
	- usado para mostrar quais testes usam aquela expressão ou para saber se tem mais de uma
	- pytest -k "any" --collect-only

    	- irá mostrar onde estão e quais são 
```
pytest -k "asdict or defaults" --collect-only                                                                                          [5d6h54m]
================================================================== test session starts ===================================================================
platform linux -- Python 3.8.3, pytest-6.2.2, py-1.10.0, pluggy-0.13.1
rootdir: /home/serlus/repositorios_estudos/pytest_code/ch1
collected 6 items / 4 deselected / 2 selected                                                                                                            

<Module tasks/test_four.py>
  <Function test_asdict>
<Module tasks/test_three.py>
  <Function test_defaults>

====================================================== 2/6 tests collected (4 deselected) in 0.02s =======================================================
```
- pytest -k
	- sem o collect ele irá rodar os testes q tem a expressão e mostrar o resultado
```
pytest -k "asdict or defaults"                                                                                                          [5d7h5m]
================================================================== test session starts ===================================================================
platform linux -- Python 3.8.3, pytest-6.2.2, py-1.10.0, pluggy-0.13.1
rootdir: /home/serlus/repositorios_estudos/pytest_code/ch1
collected 6 items / 4 deselected / 2 selected                                                                                                            

tasks/test_four.py .                                                                                                                               [ 50%]
tasks/test_three.py .                                                                                                                              [100%]

============================================================ 2 passed, 4 deselected in 0.02s =============================================================
```
- pytest -v -k 
	- mas como saber se são os testes certos? basta acrescentar o verbose
```
pytest -v -k "asdict or defaults"                                                                                                       [5d7h5m]
================================================================== test session starts ===================================================================
platform linux -- Python 3.8.3, pytest-6.2.2, py-1.10.0, pluggy-0.13.1 -- /home/serlus/repositorios_estudos/pytest_code/venv/bin/python
cachedir: .pytest_cache
rootdir: /home/serlus/repositorios_estudos/pytest_code/ch1
collected 6 items / 4 deselected / 2 selected                                                                                                            

tasks/test_four.py::test_asdict PASSED                                                                                                             [ 50%]
tasks/test_three.py::test_defaults PASSED                                                                                                          [100%]

============================================================ 2 passed, 4 deselected in 0.02s =============================================================
```
- -m MARKEXPR
	- é usada para rodar os testes q foram marcar usando um decoretor sobre eles @pytest.mark.roda_esse_test por exemplo. Se o mark não for padrão vc pode acrescentar eles nas configurações
```
pytest -v -m run_these_please                                                                                                      
================================================================== test session starts ===================================================================
platform linux -- Python 3.8.3, pytest-6.2.2, py-1.10.0, pluggy-0.13.1 -- /home/serlus/repositorios_estudos/pytest_code/venv/bin/python
cachedir: .pytest_cache
rootdir: /home/serlus/repositorios_estudos/pytest_code/ch1/tasks
collected 4 items / 2 deselected / 2 selected                                                                                                            

test_four.py::test_replace PASSED                                                                                                                  [ 50%]
test_three.py::test_member_access PASSED                                                                                                           [100%]

==================================================================== warnings summary ====================================================================
test_four.py:18
  /home/serlus/repositorios_estudos/pytest_code/ch1/tasks/test_four.py:18: PytestUnknownMarkWarning: Unknown pytest.mark.run_these_please - is this a typo?  You can register custom marks to avoid this warning - for details, see https://docs.pytest.org/en/stable/mark.html
    @pytest.mark.run_these_please

test_three.py:16
  /home/serlus/repositorios_estudos/pytest_code/ch1/tasks/test_three.py:16: PytestUnknownMarkWarning: Unknown pytest.mark.run_these_please - is this a typo?  You can register custom marks to avoid this warning - for details, see https://docs.pytest.org/en/stable/mark.html
    @pytest.mark.run_these_please

-- Docs: https://docs.pytest.org/en/stable/warnings.html
====================================================== 2 passed, 2 deselected, 2 warnings in 0.03s =======================================================
```
- -x, --exitfirst
  
	- irá parar na primeira falha. É útil resolver um problema por vez.
```
pytest -x                                                                                                                            [5d7h39m] ✹
================================================================== test session starts ===================================================================
platform linux -- Python 3.8.3, pytest-6.2.2, py-1.10.0, pluggy-0.13.1
rootdir: /home/serlus/repositorios_estudos/pytest_code/ch1
collected 6 items                                                                                                                                        

test_one.py .                                                                                                                                      [ 16%]
test_two.py F

======================================================================== FAILURES ========================================================================
______________________________________________________________________ test_failing ______________________________________________________________________

    def test_failing():
>       assert (1, 2, 3) == (3, 2, 1)
E       assert (1, 2, 3) == (3, 2, 1)
E         At index 0 diff: 1 != 3
E         Use -v to get the full diff

test_two.py:2: AssertionError

================================================================ short test summary info =================================================================
FAILED test_two.py::test_failing - assert (1, 2, 3) == (3, 2, 1)
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! stopping after 1 failures !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
======================================================== 1 failed, 1 passed, 2 warnings in 0.05s =========================================================
```
Ele ainda coleta todos os testes mas mostra o primeiro q passou e já mostra onde esta dando o primeiro erro.
É possível rodar apenas as falhas escondendo o traceback deixando a tela mais limpa

- --maxfail=num
  
	- pytest --maxfail=2
	- Similar ao -x mas agora estamos pedindo q retorne um máximo de erros q vamos indicar depois do maxfail

```
pytest --maxfail=1 --tb=no                                                                                                         
================================================================== test session starts ===================================================================
platform linux -- Python 3.8.3, pytest-6.2.2, py-1.10.0, pluggy-0.13.1
rootdir: /home/serlus/repositorios_estudos/pytest_code/ch1
collected 6 items                                                                                                                                        

test_one.py .                                                                                                                                      [ 16%]
test_two.py F


================================================================ short test summary info =================================================================
FAILED test_two.py::test_failing - assert (1, 2, 3) == (3, 2, 1)
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! stopping after 1 failures !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
======================================================== 1 failed, 1 passed, 2 warnings in 0.02s =========================================================
```

- -s and --capture=method
  
	- usado para omitir print nos testes
- --lf, --last-failed
	- Esse é um método muito útil para rodar o último teste e apenas ele, para começar o debuging
```
pytest --lf                                                                                                                         ⏎ [5d8h6m] ✹
================================================================== test session starts ===================================================================
platform linux -- Python 3.8.3, pytest-6.2.2, py-1.10.0, pluggy-0.13.1
rootdir: /home/serlus/repositorios_estudos/pytest_code/ch1
collected 1 item                                                                                                                                         
run-last-failure: rerun previous 1 failure (skipped 3 files)

test_two.py F                                                                                                                                      [100%]

======================================================================== FAILURES ========================================================================
______________________________________________________________________ test_failing ______________________________________________________________________

    def test_failing():
>       assert (1, 2, 3) == (3, 2, 1)
E       assert (1, 2, 3) == (3, 2, 1)
E         At index 0 diff: 1 != 3
E         Use -v to get the full diff

test_two.py:2: AssertionError
================================================================ short test summary info =================================================================
FAILED test_two.py::test_failing - assert (1, 2, 3) == (3, 2, 1)
=================================================================== 1 failed in 0.05s ====================================================================
```
- --ff, --failed-first
	- por conta teste ter falhando antes ele organizar os testes q falharam por primeiro
- -v, --verbose
	- ele mostra pelo nome das coisas. verbose pode ser usado sempre em conjunto com outras opções como ff ou tb
```
pytest -v --ff --tb=no                                                                                                             ⏎ [5d8h15m] ✹
================================================================== test session starts ===================================================================
platform linux -- Python 3.8.3, pytest-6.2.2, py-1.10.0, pluggy-0.13.1 -- /home/serlus/repositorios_estudos/pytest_code/venv/bin/python
cachedir: .pytest_cache
rootdir: /home/serlus/repositorios_estudos/pytest_code/ch1
collected 6 items                                                                                                                                        
run-last-failure: rerun previous 1 failure first

test_two.py::test_failing FAILED                                                                                                                   [ 16%]
test_one.py::test_passing PASSED                                                                                                                   [ 33%]
tasks/test_four.py::test_asdict PASSED                                                                                                             [ 50%]
tasks/test_four.py::test_replace PASSED                                                                                                            [ 66%]
tasks/test_three.py::test_defaults PASSED                                                                                                          [ 83%]
tasks/test_three.py::test_member_access PASSED                                                                                                     [100%]

================================================================ short test summary info =================================================================
FAILED test_two.py::test_failing - assert (1, 2, 3) == (3, 2, 1)
======================================================== 1 failed, 5 passed, 2 warnings in 0.05s =========================================================
```
- -q, --quiet
	- é em oposição ao verbose, ele remove todas a formas declarativas dos testes, colocando apenas em uma linha os sinais dos testes. 
```
pytest -q                                                                                                                          ⏎ [5d8h26m] ✹
.F....                                                                                                                                             [100%]
======================================================================== FAILURES ========================================================================
______________________________________________________________________ test_failing ______________________________________________________________________

    def test_failing():
>       assert (1, 2, 3) == (3, 2, 1)
E       assert (1, 2, 3) == (3, 2, 1)
E         At index 0 diff: 1 != 3
E         Full diff:
E         - (3, 2, 1)
E         ?  ^     ^
E         + (1, 2, 3)
E         ?  ^     ^

test_two.py:2: AssertionError

================================================================ short test summary info =================================================================
FAILED test_two.py::test_failing - assert (1, 2, 3) == (3, 2, 1)
1 failed, 5 passed, 2 warnings in 0.07s
```
- junto com tb=line pode ser muito util para não poluir tanto o retorno
```
pytest -q --tb=line                                                                                                                ⏎ [5d8h30m] ✹
.F....                                                                                                                                             [100%]
======================================================================== FAILURES ========================================================================
/home/serlus/repositorios_estudos/pytest_code/ch1/test_two.py:2: assert (1, 2, 3) == (3, 2, 1)
================================================================ short test summary info =================================================================
FAILED test_two.py::test_failing - assert (1, 2, 3) == (3, 2, 1)
1 failed, 5 passed, 2 warnings in 0.04s
```

- -l, --showlocals
	- usado para mostrar os teste de um local especifico, uma pasta. assim não precisa ficar entrando e saindo das pastas.
- --tb=style
	- Essa é uma forma de mudar o traceback de saida das falhas. As três mais usuais são no, line e short
	- --tb=no
	- --tb=line
	- --tb-short
- --durations=N 
	- pytest --durations=N 
	- usado para verificar o qual rápido esta seus testes. N irá indicar o tempo dos N últimos testes
```
pytest --durations=3 tasks                                                                                                            ⏎ [23h11m]
================================================================== test session starts ===================================================================
platform linux -- Python 3.8.3, pytest-6.2.2, py-1.10.0, pluggy-0.13.1
rootdir: /home/serlus/repositorios_estudos/pytest_code/ch1
collected 4 items                                                                                                                                        

tasks/test_four.py ..                                                                                                                              [ 50%]
tasks/test_three.py ..                                                                                                                             [100%]

================================================================== slowest 3 durations ===================================================================

(3 durations < 0.005s hidden.  Use -vv to show these durations.)
============================================================= 4 passed, 2 warnings in 0.02s ==============================================================
```
Mas para mostrar como citado no livro precisa usar a opção vv
```
pytest -vv --durations=3 tasks                                                                                                      ⏎ [23h21m] ✹
================================================================== test session starts ===================================================================
platform linux -- Python 3.8.3, pytest-6.2.2, py-1.10.0, pluggy-0.13.1 -- /home/serlus/repositorios_estudos/pytest_code/venv/bin/python
cachedir: .pytest_cache
rootdir: /home/serlus/repositorios_estudos/pytest_code/ch1
collected 4 items                                                                                                                                        

tasks/test_four.py::test_asdict PASSED                                                                                                             [ 25%]
tasks/test_four.py::test_replace PASSED                                                                                                            [ 50%]
tasks/test_three.py::test_defaults PASSED                                                                                                          [ 75%]
tasks/test_three.py::test_member_access PASSED                                                                                                     [100%]

================================================================== slowest 3 durations ===================================================================
0.00s setup    tasks/test_four.py::test_replace
0.00s teardown tasks/test_four.py::test_asdict
0.00s setup    tasks/test_four.py::test_asdict
============================================================= 4 passed, 2 warnings in 0.02s ==============================================================
```
Quando acrescenta um time sleep em um dos teste ele muda a chamada
```
================================================================== slowest 3 durations ===================================================================
0.10s call     tasks/test_four.py::test_replace
0.00s setup    tasks/test_three.py::test_defaults
0.00s setup    tasks/test_four.py::test_asdict
```
Com a opção 0 ele mostra as três fases de todos os testes
```
pytest -vv --durations=0 tasks
=================================================================== slowest durations ====================================================================
0.10s call     tasks/test_four.py::test_replace
0.00s call     tasks/test_three.py::test_member_access
0.00s call     tasks/test_four.py::test_asdict
0.00s setup    tasks/test_three.py::test_defaults
0.00s teardown tasks/test_three.py::test_member_access
0.00s setup    tasks/test_four.py::test_asdict
0.00s teardown tasks/test_four.py::test_replace
0.00s setup    tasks/test_four.py::test_replace
0.00s setup    tasks/test_three.py::test_member_access
0.00s teardown tasks/test_four.py::test_asdict
0.00s teardown tasks/test_three.py::test_defaults
0.00s call     tasks/test_three.py::test_defaults
```