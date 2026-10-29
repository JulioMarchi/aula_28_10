# Nexo Studio — Guia de instalação no VS Code (Windows)

Este roteiro ajuda cada grupo a executar o **Nexo Studio localmente no VS Code**, usando o arquivo ZIP disponibilizado no repositório GitHub.

> **Importante:** os comandos abaixo pressupõem que o ZIP contenha o projeto Django e o arquivo `requirements.txt`. Se a estrutura extraída for diferente, confira as pastas antes de continuar.

## 1. Baixar o projeto

1. Abra o repositório `nexo-studio` no GitHub.
2. Clique no arquivo `Nexo_Studio_Projeto_Completo_v0.zip`.
3. Baixe o arquivo para o computador.
4. Crie uma pasta de trabalho, por exemplo `C:\Projetos\NexoStudio`.
5. Copie o ZIP para essa pasta.

## 2. Abrir a pasta no VS Code

1. Abra o VS Code.
2. Acesse **File > Open Folder** (Arquivo > Abrir Pasta).
3. Selecione `C:\Projetos\NexoStudio`.
4. Abra o terminal integrado em **Terminal > New Terminal**.

## 3. Extrair o ZIP

No terminal do VS Code, execute:

```powershell
Expand-Archive -Path .\Nexo_Studio_Projeto_Completo_v0.zip -DestinationPath .\extraido
```

Esse comando extrai os arquivos para a pasta `extraido`.

Confira o conteúdo:

```powershell
Get-ChildItem .\extraido
```

Entre na pasta que contém `manage.py` e `requirements.txt`. Por exemplo, se esses arquivos estiverem diretamente em `extraido`:

```powershell
Set-Location .\extraido
```

Se estiverem em uma subpasta, entre nela usando o caminho que apareceu na listagem. **Os comandos Django devem ser executados na pasta que contém `manage.py`.**

## 4. Verificar o Python

```powershell
python --version
```

Se o comando não funcionar, o Python precisa ser instalado ou configurado no computador. Não prossiga até o terminal conseguir identificar uma versão do Python.

## 5. Criar e ativar um ambiente virtual

Execute na pasta que contém `manage.py`:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

O ambiente virtual isola as bibliotecas do projeto das demais instalações Python.

Se o PowerShell bloquear a ativação, tente no terminal Command Prompt (cmd):

```bat
.venv\Scripts\activate.bat
```

## 6. Instalar as dependências

```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

O primeiro comando atualiza o instalador de pacotes do ambiente virtual. O segundo instala as bibliotecas listadas em `requirements.txt`.

## 7. Criar o arquivo `.env`

Confira se existe um arquivo `.env.example`:

```powershell
Get-ChildItem -Force
```

Se existir, crie o `.env`:

```powershell
Copy-Item .env.example .env
```

Se não existir `.env.example`, não invente os nomes das variáveis. Peça ao professor o modelo correto de `.env` para a versão do projeto.

## 8. Gerar uma chave secreta individual

Com o ambiente virtual ativado e as dependências instaladas, execute:

```powershell
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

Copie a chave gerada. Abra o arquivo `.env` no VS Code:

```powershell
code .env
```

Localize `NEXO_SECRET_KEY` e substitua o valor de exemplo pela chave gerada. Salve com **Ctrl+S**.

**Segurança:**
- Cada grupo deve gerar sua própria chave.
- Não publique nem compartilhe o `.env`.
- Não envie o `.env` ao GitHub.
- Mantenha o `.env` fora dos materiais distribuídos aos demais grupos.

## 9. Conferir as configurações do `.env`

As variáveis exatas dependem do `config/settings.py` desta versão do projeto. Confira o modelo fornecido pelo professor e preencha os valores necessários.

Se o projeto for acessado somente pelo próprio computador, normalmente o host local é usado. Para acessar a aplicação de outro dispositivo ou por um domínio HTTPS, as configurações de host e CSRF podem precisar de ajustes específicos.

Não desative a proteção CSRF para contornar erros.

## 10. Verificar a configuração Django

Na pasta que contém `manage.py`, execute:

```powershell
python manage.py check
```

O comando verifica a configuração da aplicação. Se aparecer um erro, copie a mensagem completa para compartilhar com o professor.

## 11. Preparar o banco de dados

```powershell
python manage.py migrate
```

Esse comando cria ou atualiza as tabelas do banco de dados conforme as migrações do projeto.

## 12. Iniciar o Nexo Studio

```powershell
python manage.py runserver
```

Abra no navegador o endereço informado pelo Django, normalmente:

`http://127.0.0.1:8000/`

Para encerrar o servidor, volte ao terminal e pressione **Ctrl+C**.

## 13. Checklist de validação

- [ ] O terminal está na pasta que contém `manage.py`.
- [ ] O ambiente virtual `.venv` está ativado.
- [ ] As dependências foram instaladas.
- [ ] O arquivo `.env` foi criado e contém uma chave individual.
- [ ] `python manage.py check` foi executado.
- [ ] `python manage.py migrate` terminou sem erros.
- [ ] O servidor iniciou e o site abriu no navegador.
- [ ] O grupo testou o cadastro e as principais telas.

## Problemas comuns

### `python` não é reconhecido

Verifique a instalação e a configuração do Python no Windows. Depois, feche e reabra o terminal do VS Code.

### `manage.py` não foi encontrado

O terminal está na pasta errada. Use `Get-ChildItem` para localizar `manage.py` e entre na pasta que o contém.

### `requirements.txt` não foi encontrado

Você pode estar em uma pasta diferente da raiz do projeto. Localize o arquivo e execute `pip install -r requirements.txt` na pasta correta.

### Erro de CSRF ou host inválido

Não desative o CSRF. Confira `NEXO_ALLOWED_HOSTS` e `NEXO_CSRF_TRUSTED_ORIGINS` no `.env` e siga as orientações do professor para o ambiente local.

### O site não abre

Confira se o terminal ainda mostra o servidor em execução e se o navegador está acessando o endereço local correto.

---

**Regra de ouro:** execute um comando por vez, leia o resultado e só então passe para a próxima etapa.
