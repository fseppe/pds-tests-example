# Trabalho Prático sobre Testes de Software

#### Aluno: Felipe Seppe de Faria
#### Matrícula: 2017014596

## Teste 1
Este primeiro teste foi retirado da biblioteca [**mypy**](https://github.com/python/mypy), que é um analisador estático de código para python, auxiliando na escrita de código estaticamente tipado de Python. Diferentemente do TypeScript, o mypy não exige que o código seja compilado.

O teste pode ser encontrado [aqui](https://github.com/python/mypy/blob/22ab84acc3f6174905a05ddadab94b6c2d83f9a1/mypy/test/testformatter.py#L33).
```python
  def test_split_words(self) -> None:
      assert split_words('Simple message') == ['Simple', 'message']
      assert split_words('Message with "Some[Long, Types]"'
                         ' in it') == ['Message', 'with',
                                       '"Some[Long, Types]"', 'in', 'it']
      assert split_words('Message with "Some[Long, Types]"'
                         ' and [error-code]') == ['Message', 'with', '"Some[Long, Types]"',
                                                  'and', '[error-code]']
      assert split_words('"Type[Stands, First]" then words') == ['"Type[Stands, First]"',
                                                                 'then', 'words']
      assert split_words('First words "Then[Stands, Type]"') == ['First', 'words',
                                                                 '"Then[Stands, Type]"']
      assert split_words('"Type[Only, Here]"') == ['"Type[Only, Here]"']
      assert split_words('OneWord') == ['OneWord']
      assert split_words(' ') == ['', '']
```
Nesse teste verifica se a função de separar palavras de uma string funciona como o esperado. Apesar de ser um teste bem simples, este teste foi escolhido para exemplificar um caso de teste que pode ser melhorado. Uma vez que caso o programador receba um erro durante os testes de unidade desse método, ele não saberá explicitamente qual o caso que quebrou, já que em um mesmo teste possui diversos *assert* que testam diferentes casos do método `split_words`.  

## Teste 2
Este segundo teste foi retirado do framework [**flask**](https://github.com/pallets/flask), que é um framework web para python. Flask é leve e robusto, suportando aplicações mais complexas, oferecendo bibliotecas de suporte (como de ORM) mas sendo flexível, permitindo que o programador escolha a melhor. 
 
O teste pode ser encontrado [aqui](https://github.com/pallets/flask/blob/9486b6cf57bd6a8a261f67091aca8ca78eeec1e3/tests/test_json.py#L78).
```python
def test_jsonify_dicts(app, client):
    d = {
        "a": 0,
        "b": 23,
        "c": 3.14,
        "d": "t",
        "e": "Hi",
        "f": True,
        "g": False,
        "h": ["test list", 10, False],
        "i": {"test": "dict"},
    }

    @app.route("/kw")
    def return_kwargs():
        return flask.jsonify(**d)

    @app.route("/dict")
    def return_dict():
        return flask.jsonify(d)

    for url in "/kw", "/dict":
        rv = client.get(url)
        assert rv.mimetype == "application/json"
        assert flask.json.loads(rv.data) == d
```
Como é um framework web, flask precisa ser flexível quanto à maneira que retorna as informações requisitadas na web. Portanto, este teste verifica se o framework está transformando um dicionário adequadamente para JSON, uma vez que fazem uso de um próprio wrapper para a criação de JSON. 

Neste teste, a função é testada simulada a requisição web de um cliente e por isso, não somente a conversão do dicionário está sendo testada, mas também se está retornando adequadamente o tipo de dado `"application/json"`. Para verificar se o dicionário foi convertido corretamente, o resultado é testado usando a função de carregamento de um JSON e o dicionário original.

É interessante notar que neste teste, é feito apenas o teste de conversão dos tipos string, dict, list, int, float e bool. Aparentemente, tipos mais complexos como Number não é suportado.

## Teste 3
Este terceiro teste, bem como o segundo teste, foi retirado do framework [**flask**](https://github.com/pallets/flask).

O teste pode ser encontrado [aqui](https://github.com/pallets/flask/blob/9486b6cf57bd6a8a261f67091aca8ca78eeec1e3/tests/test_basic.py#L232).
```python
def test_session(app, client):
    @app.route("/set", methods=["POST"])
    def set():
        assert not flask.session.accessed
        assert not flask.session.modified
        flask.session["value"] = flask.request.form["value"]
        assert flask.session.accessed
        assert flask.session.modified
        return "value set"

    @app.route("/get")
    def get():
        assert not flask.session.accessed
        assert not flask.session.modified
        v = flask.session.get("value", "None")
        assert flask.session.accessed
        assert not flask.session.modified
        return v

    assert client.post("/set", data={"value": "42"}).data == b"value set"
    assert client.get("/get").data == b"42"
```
Este teste é bem claro quanto ao comportamento esperado do componente `session` do flask, apesar de que o nome do teste pudesse ser mais claro. Uma vez que testa o `set` e `get` do session, bem como a persistência dos dados entre chamadas do cliente. 

Neste teste é verificado se informações não persistentes estão funcionando adequadamente, como o `accessed` e `modified`, por isso são verificados tanto no `set` quanto no `get`. Portanto, os dois últimos *assert* do código verificam se o `set` foi executado adequadamente e retornou a mensagem de finalização e no `get` se o valor salvo pelo `set` persistiu e se foi salvo adequadamente.
